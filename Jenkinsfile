properties([parameters([booleanParam(defaultValue: true, description: 'select to format the code', name: 'Autopep8')])])
node {
    stage('Building Docker image'){
        git 'git@github.com:lyogi4091/Ex-3.git'
        sh 'sudo docker build -t ubuntu_image_remotely .'
        sh 'sudo docker run -it -d --name testcontainer_remote_ex_3 -v /home/ciuser/Ex-3:/opt ubuntu_image_remotely'
        try{
            sh 'sudo docker exec testcontainer_remote_ex_3 cat /opt/python_good.py'
            sh 'sudo docker exec testcontainer_remote_ex_3 pycodestyle --ignore=E501 /opt/python_good.py'
            println "Code is already in good format"
            }catch(x){
                if (Boolean.parseBoolean("${env.Autopep8}")) {
                    sh 'sudo docker exec testcontainer_remote_ex_3 autopep8 -i /opt/python_good.py'
                    sh 'sudo docker exec testcontainer_remote_ex_3 cat /opt/python_good.py'
                    println "Code is now formatted."
                }
                }
        try{
            sh 'sudo docker exec testcontainer_remote_ex_3 cat /opt/python_bad.py'
            sh 'sudo docker exec testcontainer_remote_ex_3 pycodestyle --ignore=E501 /opt/python_bad.py'
            println "Code is already in good format"
            }catch(x){
                if (Boolean.parseBoolean("${env.Autopep8}")){
                    sh 'sudo docker exec testcontainer_remote_ex_3 autopep8 -i /opt/python_bad.py'
                    sh 'sudo docker exec testcontainer_remote_ex_3 cat /opt/python_bad.py'
                    println "Code is now formatted"
                }
                }
    stage('Pushing the formatted code'){
        dir('/home/ciuser/Exercise-3'){
            try{
                sh 'sudo git config --global user.email "lingojuyogesh.kumar@ltts.com"';
                sh 'sudo git config --global user.name "Yogesh Kumar"'
                sh 'sudo git add python_bad.py';
                sh 'sudo git commit -m "Commit after autopep8"';
                sh 'sudo git push origin master';
                }catch (d){
                echo "No changes found to push"
        }
    }
    }
}
}
