pipeline {
  environment {
    // อย่าลืมสร้าง Credentials ชื่อ vercel-token ใน Jenkins ก่อนนะ
    VERCEL_TOKEN = credentials('vercel-token-exam') 
    
    // ใส่ ID ของน้อง (จากไฟล์ .vercel/project.json)
    VERCEL_ORG_ID = 'team_SgJYI9s7kKGMxPAXCdhyPpGl' 
    VERCEL_PROJECT_ID = 'prj_qGX5c6DVKvQHT5Xbr6jFmJ516S17'
  }
  
  // ใช้ Docker Agent เพราะรันบนเครื่องเรา
  agent {
    docker {
      image 'node:20-alpine'
      args '-v /root/.npm:/root/.npm'
    }
  }

  stages {
    stage('Test npm') {
      steps {
        sh 'node --version'
        sh 'npm --version'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm ci' 
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Deploy to Vercel') {
      steps {
        sh 'npm install -g vercel'
        // Deploy แบบ Pro
        sh """
           vercel --prod --token=${VERCEL_TOKEN} --yes
        """
      }
    }
  }
}