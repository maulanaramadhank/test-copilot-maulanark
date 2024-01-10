pipeline {
    agent any
    
    stages {
        stage('Configure gcloud credentials') {
            steps {
                sh 'curl -o service-account-key.json https://gitlab.com/Harumasanada/json-cred/-/raw/main/mythic-hulling-407902-f617609ea4bd.json?ref_type=heads&inline=false'
                sh 'gcloud auth activate-service-account --key-file=service-account-key.json'
            }
            steps {
                    def responseMessage = "Build successful! This is Configure gcloud credentials."
                    echo "POSTMAN_RESPONSE_MESSAGE=${responseMessage}"
            }
        }
        
        stage('Set project and zone') {
            steps {
                sh 'gcloud config set project mythic-hulling-407902'
                sh 'gcloud config set compute/zone us-west4-b'
            }
            steps {
                    def responseMessage = "Project and zone successfully applied."
                    echo "POSTMAN_RESPONSE_MESSAGE=${responseMessage}"
            }
        }
        
        stage('Create VM instance') {
            steps {
                        def responseMessage = "Creating VM instance ......"
                        echo "POSTMAN_RESPONSE_MESSAGE=${responseMessage}"
            }
            environment {
                INSTANCE_NAME = "my-instance"
                MACHINE_TYPE = "e2-micro"
                OS_PATH = "projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20231213"
                DISK_SIZE = "10"
            }
            
            steps {
                sh 'gcloud compute instances create $INSTANCE_NAME \
                    --project=mythic-hulling-407902 \
                    --zone=us-west4-b \
                    --machine-type=$MACHINE_TYPE \
                    --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default \
                    --maintenance-policy=MIGRATE \
                    --provisioning-model=STANDARD \
                    --service-account=156933639626-compute@developer.gserviceaccount.com \
                    --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append \
                    --tags=http-server,https-server \
                    --create-disk=auto-delete=yes,boot=yes,device-name=$INSTANCE_NAME,image=$OS_PATH,mode=rw,size=$DISK_SIZE,type=projects/mythic-hulling-407902/zones/us-west4-b/diskTypes/pd-standard \
                    --no-shielded-secure-boot \
                    --shielded-vtpm \
                    --shielded-integrity-monitoring \
                    --labels=goog-ec-src=vm_add-gcloud \
                    --reservation-affinity=any'
            }
            steps {
                        def responseMessage = "VM instace created successfully."
                        echo "POSTMAN_RESPONSE_MESSAGE=${responseMessage}"
            }
        }
    }
}
