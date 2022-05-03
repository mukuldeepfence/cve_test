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
    
    stage('which user is in use') {
        sh "package-scanner -deepfence-key=42c05cb2-c03e-4a49-9e3a-2347a278261d -vulnerability-scan=true -output=json -mode=local -mgmt-console-url=157.245.15.65 -source=ubuntu:16.04 -fail-on-count=5000 -fail-on-score=-1 -mask-cve-ids=''"
        sh "cat /etc/group"
        sh "ls -lah /var/run/docker.sock"
    }

    // stage('Build image') {
       // app = docker.build("${full_image_name}", "-f Dockerfile .")
    // }
    
    stage('Build image') {
        sh "docker build -t test -f Dockerfile ."
    }

    // stage('Remove unused docker image') {
        // sh "docker rmi ${full_image_name}"
    // }
}
