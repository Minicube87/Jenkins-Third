pipeline {
  agent {
    docker { image 'ubuntu:20.04' }
  }

  stages {

    stage('Build') {
      steps {
        echo 'Starte Build...'
        sh 'echo "Dies ist ein Testfile" > ergebnis.txt'
        echo 'Datei erstellt.'
      }
    }

    stage('Test') {
      steps {
        echo 'Prüfe Dateiinhalt...'
        // grep sucht nach dem Wort "Testfile" in der Datei
        sh '''
          if grep -q "Testfile" ergebnis.txt; then
            echo "Test erfolgreich!"
          else
            echo "Test fehlgeschlagen!"
            exit 1
          fi
        '''
      }
    }

    stage('Cleanup') {
      steps {
        echo 'Lösche temporäre Datei...'
        sh 'rm -f ergebnis.txt'
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