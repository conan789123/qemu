// import the cheribuildProject() step
library 'ctsrd-jenkins-scripts'

def archiveQEMU(String target) {
	return {
		sh "rm -rf \$WORKSPACE/qemu-${target} && mv \$WORKSPACE/tarball/usr \$WORKSPACE/qemu-${target}"
		archiveArtifacts allowEmptyArchive: false, artifacts: "qemu-${target}/bin/qemu-system-*, qemu-${target}/share/qemu/efi-pcnet.rom, qemu-${target}/share/qemu/vgabios-cirrus.bin", fingerprint: true, onlyIfSuccessful: true
	}
}
  
cheribuildProject(target: 'qemu', cpu: 'native', skipArtifacts: true,
      buildStage: "Build Linux", nodeLabel: 'linux', noIncrementalBuild: true,
      skipScm: true, beforeBuild: "rm -rf qemu; git clone --reference /var/tmp/git-reference-repos/qemu https://github.com/CTSRD-CHERI/qemu.git qemu",
      extraArgs: '--unified-sdk --without-sdk --install-prefix=/usr --qemu/legacy-registers',
      skipTarball: true, afterBuild: archiveQEMU('linux'))

cheribuildProject(target: 'qemu', cpu: 'native', skipArtifacts: true,
      buildStage: "Build FreeBSD", nodeLabel: 'freebsd', noIncrementalBuild: true,
      skipScm: true, beforeBuild: "rm -rf qemu; git clone --reference /var/tmp/git-reference-repos/qemu https://github.com/CTSRD-CHERI/qemu.git qemu",
      extraArgs: '--unified-sdk --without-sdk --install-prefix=/usr --qemu/legacy-registers',
      skipTarball: true, afterBuild: archiveQEMU('freebsd'))
