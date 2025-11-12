pipeline {
  agent {
    docker { image 'ubuntu:20.04' }
  }

  parameters {
    string(name: 'FILENAME', defaultValue: 'ergebnis.txt', description: 'Name der Ausgabedatei')
    choice(name: 'TESTWORD', choices: ['Testfile', 'Build', 'CI'], description: 'Gesuchtes Wort')
    booleanParam(name: 'DO_CLEANUP', defaultValue: true, description: 'Datei zum Schluss löschen?')
  }

  stages {

    stage('Build') {
      steps {
        echo 'Starte Build...'
        sh "echo 'Dies ist ein ${TESTWORD}' > ${FILENAME}"
        echo "Datei '${FILENAME}' erstellt."
      }
    }

    stage('Test') {
      steps {
        echo 'Prüfe Dateiinhalt...'
        // grep sucht nach dem Wort "Testfile" in der Datei
        sh '''
          if grep -q "${TESTWORD}" ${FILENAME} then
            echo "Test erfolgreich!"
          else
            echo "Test fehlgeschlagen!"
            exit 1
          fi
        '''
      }
    }

    stage('Cleanup') {
        when {
            expression { params.DO_CLEANUP }  // das hier wird nur ausgeführt wenn true
        }
      steps {
        echo "Lösche '${FILENAME}'..."
        sh "rm -f ${FILENAME}"
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