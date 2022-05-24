node {
    def app
    def full_image_name = 'deepfenceio/jenkins-example:latest'
    def deepfence_mgmt_console_url = '137.184.10.129' // URL address of Deepfence management console Note - Please do not mention port 
    def fail_cve_count = 252 // Fail jenkins build if number of vulnerabilities found is >= this number. Set -1 to pass regardless of vulnerabilities.
    def fail_critical_cve_count = 31 // Fail jenkins build if number of critical vulnerabilities found is >= this number. Set -1 to pass regardless of critical vulnerabilities.
    def fail_high_cve_count = 93 // Fail jenkins build if number of high vulnerabilities found is >= this number. Set -1 to pass regardless of high vulnerabilities.
    def fail_medium_cve_count = 109 // Fail jenkins build if number of medium vulnerabilities found is >= this number. Set -1 to pass regardless of medium vulnerabilities.
    def fail_low_cve_count = 19 // Fail jenkins build if number of low vulnerabilities found is >= this number. Set -1 to pass regardless of low vulnerabilities.            
    def fail_cve_score = -1 // Fail jenkins build if cumulative CVE score is >= this value. Set -1 to pass regardless of cve score.
    def mask_cve_ids = "" // Comma separated. Example: "CVE-2019-9168,CVE-2019-9169"
    def deepfence_key = "b14a5c6a-6376-454a-88af-d54b2d11de9c" // API key can be found on settings page of the deepfence 

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${full_image_name}", "-f Dockerfile .")
    }

    stage('Run Deepfence Vulnerability Mapper') {
        sh "package-scanner -deepfence-key=${deepfence_key} -vulnerability-scan=true -output=table -mode=local -mgmt-console-url=${deepfence_mgmt_console_url} -source=${full_image_name} -fail-on-count=${fail_cve_count} -fail-on-critical-count=${fail_critical_cve_count} -fail-on-high-count=${fail_high_cve_count} -fail-on-medium-count=${fail_medium_cve_count} -fail-on-low-count=${fail_low_cve_count} -fail-on-score=${fail_cve_score} -mask-cve-ids=''"
    }

    stage('Remove unused docker image') {
        sh "docker rmi ${full_image_name}"
    }
}
