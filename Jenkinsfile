pipeline{
  agent{
    label debianhost
  }

  environment {
        DB_USERNAME =  'plantillasBE'
        DB_PASSWORD =  'passwordBE'
        DB_DATABASE = 'databseBE'
        DB_HOST = 'localhost'
        DB_PORT = '5432'
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
        sh "pqsl -c CREATE USER $DB_USERNAME WITH PASSWORD '$DB_PASSWORD';";
        
        sh "su -";
        sh "sudo /etc/init.d/postgresql restart";

        //creando la base de datos
        sh "sudo su";
        sh "su postgres";
        
        sh "psql -c CREATE DATABASE $DB_DATABASE OWNER $DB_USERNAME;";
        //instalando extensiones necesarias
        sh "psql -d $DB_DATABASE -c CREATE EXTENSION IF NOT EXISTS tablefunc;";

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

        sh "sed -i 's/"username": "postgres"/"username": "$DB_USERNAME"/g' /src/config/config.json";
        sh "sed -i 's/"password": "postgres"/"password": "$DB_PASSWORD"/g' /src/config/config.json";
        sh "sed -i 's/"database": "name_database"/"database": "$DB_DATABASE"/g' /src/config/config.json";
        sh "sed -i 's/"host": "127.0.0.1"/"host": "$DB_HOST"/g' /src/config/config.json";
        sh "sed -i 's/"port": 5432/"port": "$DB_PORT"/g' /src/config/config.json";
        
      }
    }
    stage('7. instalar dependencias e inicializar db'){
      steps{
        sh "npm i";
        sh "npm run parchar";
        sh "npm run setup";
      }
    }
    stage('8. ejecucion de la aplicacion'){
      steps{
        sh "npm i -g pm2";
        sh "pm2 start prod.json";
        sh  "pm2 list";

        //completar: pm2 startup, un comando, pm2 save
      }
    }
    
  }
  
}
