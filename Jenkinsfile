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

    // stage('Remove unused docker image') {
        // sh "docker rmi ${full_image_name}"
    // }
}
