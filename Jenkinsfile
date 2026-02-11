pipeline {
  environment {
    // 1. ไปสร้าง Credentials ชนิด Secret text ใน Jenkins ชื่อนี้ก่อน
    VERCEL_TOKEN = credentials('vercel-token-exam') 
    
    // 2. เอา Project ID & Org ID จากไฟล์ .vercel/project.json ในเครื่องน้องมาใส่
    VERCEL_ORG_ID = 'team_SgJYI9s7kKGMxPAXCdhyPpGl' 
    VERCEL_PROJECT_ID = 'prj_qGX5c6DVKvQHT5Xbr6jFmJ516S17'
  }
  
  // เปลี่ยนจาก kubernetes เป็น docker (เพื่อให้รันบนเครื่องน้องได้)
  agent {
    docker {
      image 'node:20-alpine'
      // map volume เพื่อเก็บ cache npm (จะได้โหลดเร็วๆ)
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
        // ใช้ npm ci แทน install (ดีกว่าสำหรับ CI/CD)
        sh 'npm ci' 
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Test Build') {
      steps {
        // ถ้าโปรเจกต์น้องไม่มี test ให้ comment บรรทัดนี้ไว้ก่อน ไม่งั้นแดง
        // sh 'npm run test' 
        echo 'Skipping test for exam...'
      }
    }

    stage('Deploy to Vercel') {
      steps {
        // ติดตั้ง Vercel CLI
        sh 'npm install -g vercel'
        
        // Deploy แบบ Pro: ไม่ต้อง link แต่ยัด ID เข้าไปเลย
        // สังเกต: พี่ใช้ """ (ฟันหนู 3 อัน) เพื่อให้ตัวแปรทำงาน
        sh """
           vercel --prod --token=${VERCEL_TOKEN} --yes
        """
      }
    }
  }

//   post {
//     always {
//        ถ้าไม่มีไฟล์ xml ให้ปิดตรงนี้ไปก่อน ไม่งั้น pipeline จะ error ตอนจบ
//        junit 'test-results/junit.xml'
//     }
//   }
}