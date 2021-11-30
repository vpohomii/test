node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

     stage('sleepy') {
    
    sh  ''' apt-get update && apt-get install -y apt-transport-https ca-certificates software-properties-common wget && wget https://download.docker.com/linux/debian/dists/bullseye/pool/stable/amd64/docker-ce-cli_20.10.10~3-0~debian-bullseye_amd64.deb && wget https://download.docker.com/linux/debian/dists/bullseye/pool/stable/amd64/docker-ce_20.10.11~3-0~debian-bullseye_amd64.deb && wget https://download.docker.com/linux/debian/dists/bullseye/pool/stable/amd64/containerd.io_1.4.12-1_amd64.deb && dpkg -i containerd.io_1.4.12-1_amd64.deb && dpkg -i docker-ce_20.10.11~3-0~debian-bullseye_amd64.deb'''
    }
    
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("vnp79/demo")
    }
}
