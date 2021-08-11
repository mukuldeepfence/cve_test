node {
    def app
    def full_image_name = 'deepfenceio/jenkins-example:latest'
    def deepfence_key = "0f8ef5d7-3532-4d9d-a3bc-245bf2380b20" // If authentication is enabled in management console, set deepfence key here
    def deepfence_mgmt_console_ip = '147.182.185.164' // IP address of Deepfence management console
    def fail_cve_count = 100 // Fail jenkins build if number of vulnerabilities found is >= this number. Set -1 to pass regardless of vulnerabilities.
    def fail_cve_score = 8 // Fail jenkins build if cumulative CVE score is >= this value. Set -1 to pass regardless of cve score.
    def mask-cve-ids = "Path Traversal in package_data,CVE-2019-9169"

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${full_image_name}", "-f Dockerfile .")
    }

    stage('Run Deepfence Vulnerability Mapper'){
        DeepfenceAgent = docker.image("deepfenceio/deepfence_vulnerability_mapper:mask_cve")
        try {
            c = DeepfenceAgent.run("-it --net=host --privileged=true --cpus='0.3' -v /var/run/docker.sock:/var/run/docker.sock:rw", "-mgmt-console-ip='${deepfence_mgmt_console_ip}' -image-name='${full_image_name}' -deepfence-key='${deepfence_key}' -fail-cve-count=${fail_cve_count} -fail-cve-score=${fail_cve_score} -scan-type='base,java,python,ruby,php,nodejs,js,dotnet' -mask-cve-ids='${mask-cve-ids}'")
            sh "docker logs -f ${c.id}"
            def out = sh script: "docker inspect ${c.id} --format='{{.State.ExitCode}}'", returnStdout: true
            sh "exit ${out}"
        } finally {
            c.stop()
        }
    }

    stage('Remove unused docker image') {
        sh "docker rmi ${full_image_name}"
    }
}
