import os
import fsenv

DIRECTORIES = [
    'src',
    'test',
    'components/nwutil' ]

TARGET_DEFINES = {
    'freebsd_amd64': [],
    'linux32': ['_FILE_OFFSET_BITS=64'],
    'linux64': [],
    'linux_arm64': [],
    'openbsd_amd64': [],
    'darwin': []
}

TARGET_FLAGS = {
    'freebsd_amd64': '',
    'linux32': '-m32 ',
    'linux64': '',
    'linux_arm64': '',
    'openbsd_amd64': '',
    'darwin': ''
}

TARGET_FRAMEWORKS = {
    'freebsd_amd64': [],
    'linux32': [],
    'linux64': [],
    'linux_arm64': [],
    'openbsd_amd64': [],
    'darwin': ['CoreServices']
}

def construct():
    ccflags = '-g -O2'
    prefix = ARGUMENTS.get('prefix', '/usr/local')
    for target_arch in fsenv.target_architectures():
        arch_env = Environment(
            NAME='nwutil',
            ARCH=target_arch,
            PREFIX=prefix,
            PKG_CONFIG_LIBS=['fsdyn', 'encjson'],
            CCFLAGS=TARGET_FLAGS[target_arch] + ccflags,
            CPPDEFINES=TARGET_DEFINES[target_arch],
            LINKFLAGS=TARGET_FLAGS[target_arch],
            TARGET_FRAMEWORKS=TARGET_FRAMEWORKS[target_arch],
            tools=['default', 'textfile', 'fscomp'])
        fsenv.consider_environment_variables(arch_env)
        if target_arch == "darwin":
            arch_env.AppendENVPath("PATH", "/opt/local/bin")
        build_dir = os.path.join(
            fsenv.STAGE,
            target_arch,
            ARGUMENTS.get('builddir', 'build'))
        for directory in DIRECTORIES:
            env = arch_env.Clone()
            SConscript(dirs=directory,
                       exports=['env'],
                       duplicate=False,
                       variant_dir=os.path.join(build_dir, directory))
        Clean('.', build_dir)

if __name__ == 'SCons.Script':
    construct()
