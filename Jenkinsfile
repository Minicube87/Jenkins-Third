pipeline {
  agent {
    docker { image 'ubuntu:20.04' }
  }

  parameters {
    string(name: 'FILENAME', defaultValue: 'ergebnis.txt', description: 'Name der Ausgabedatei')
    string(name: 'TESTWORD', defaultValue: 'Testfile', description: 'Gesuchtes Wort')
    booleanParam(name: 'DO_CLEANUP', defaultValue: true, description: 'Datei zum Schluss löschen?')
  }

  stages {

    stage('Build') {
      steps {
        echo 'Starte Build...'
        sh "echo 'Dies ist ein ${params.TESTWORD}' > ${params.FILENAME}"
        echo "Datei '${params.FILENAME}' erstellt."
      }
    }

    stage('Test') {
      steps {
        echo 'Prüfe Dateiinhalt...'
        // grep sucht nach dem Wort "Testfile" in der Datei
        sh '''
          if grep -q ${params.TESTWORD} ${params.FILENAME}; then
            echo "Test erfolgreich!"
          fi
        '''
      }
    }

    stage('Cleanup') {
        when {
            expression { params.DO_CLEANUP }  // das hier wird nur ausgeführt wenn true
        }
      steps {
        echo "Lösche '${params.FILENAME}'..."
        sh "rm -f ${params.FILENAME}"
      }
    }
  }

  post {
    always {
      echo 'Pipeline ist durchgelaufen (egal ob Erfolg oder Fehler).'
    }
    failure {
      echo 'Es gab einen Fehler im Build oder Test!'
    }
  }
}