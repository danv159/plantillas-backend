pipeline{
  agent{
    label nodo-backend
  }
  stages{
    stage('1.instalacion de paquetes minimos'){
      steps{
        sh "sudo apt-get update";
        sh "sudo apt-get install build-essential libssl-dev";
        sh "sudo apt-get install curl";
        sh "sudo apt-get install ca-certificates";
        sh "sudo apt-get install libfontconfig1-dev libfreetype6-dev fontconfig";
        sh "sudo apt-get install unzip";
        sh "sudo apt-get install git";
        sh "sudo apt-get install openjdk-8-jdk-headless";

        sh "sudo apt -y install slapd ldap-utils";

        //configurar ldap

        
        
      }
    }
    stage('2. instalacion posqtresql'){
      steps{
        sh "sudo apt-get install postgresql-9.6";
        sh "sudo su";
        sh "su postgres";
        sh "pqsl -c CREATE USER miUsuario WITH PASSWORD 'password';";
        
        sh "su -";
        sh "sudo /etc/init.d/postgresql restart";

        //creando la base de datos
        sh "sudo su";
        sh "su postgres";
        
        sh "psql -c CREATE DATABASE miBaseDeDatos OWNER miUsuario;";
        //instalando extensiones necesarias
        sh "psql -d miBaseDeDatos -c CREATE EXTENSION IF NOT EXISTS tablefunc;";

        sh "psql -c  "\l" ";
        
        
        
      }
    }
    stage('3. instalacion de node, node version manager'){
      steps{
        sh "curl https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash";
        sh "nvm --version";

        sh "nvm install 12.22.7";
        sh "nvm use 12.22.7";
        sh "node --version";

        sh "npm i -g sequelize sequelize-cli";
        sh "npm i -g pg pg-hstore;
        sh "npm i -g apidoc";
        sh "npm i -g nodemon";

        
                
      }
    }
    stage('4. descargar el proyecto'){
      steps{
        sh "git clone https://github.com/danv159/plantillas-backend.git";
        sh "cd plantillas-backend.git";
      }
    }
    stage('5. instalacion de la fuente'){
      steps{
        sh "sudo mkdir /usr/share/fonts/truetype/opensans";

        sh "sudo cp recursos/open-sans.zip /usr/share/fonts/truetype/opensans";
        sh "cd /usr/share/fonts/truetype/opensans";
        sh "sudo unzip open-sans.zip";
        sh "cd -";
        
      }
    }
    stage('6. archivos de configuracion'){
      steps{
        sh "cp src/config/config.json.sample src/config/config.json";
        sh "cp src/config/config.js.sample src/config/config.js";
      }
    }
    stage(''){
      steps{
        
      }
    }
    stage(''){
      steps{
        
      }
    }
    stage(''){
      steps{
        
      }
    }
  }
  
}
