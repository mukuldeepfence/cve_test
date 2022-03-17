node {
    def app
    def full_image_name = 'ubuntu:latest'
    def deepfence_mgmt_console_url = '137.184.52.247' // URL address of Deepfence management console
    def fail_cve_count = 300 // Fail jenkins build if number of vulnerabilities found is >= this number. Set -1 to pass regardless of vulnerabilities.
    def fail_cve_score = 8 // Fail jenkins build if cumulative CVE score is >= this value. Set -1 to pass regardless of cve score.
    def mask_cve_ids = "" // Comma separated. Example: "CVE-2019-9168,CVE-2019-9169"

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${full_image_name}", "-f Dockerfile .")
    }

    stage('Run Deepfence Vulnerability Mapper'){
        DeepfenceAgent = docker.image("deepfenceio/deepfence_package_scanner_ce:1.3.0")
        try {
            c = DeepfenceAgent.run("-it --net=host -v /var/run/docker.sock:/var/run/docker.sock", "-deepfence-key='0817b37f-eea1-4fdb-9c93-4863b22c2e0a' -vulnerability-scan=true -output=table -mode=local -mgmt-console-url=${deepfence_mgmt_console_url} -source=${full_image_name} -fail-on-count=${fail_cve_count} -fail-on-score=${fail_cve_score} -mask-cve-ids='${mask_cve_ids}'")
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
