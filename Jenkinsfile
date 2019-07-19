node {
    stage('SCM'){
        git 'ssh://git@localhost:7999/proj/ex-3.git'
    stage('Building Docker image'){  
                sh 'sudo docker build -t ubuntu_image_remotely .';
                sh 'sudo docker run -it -d --name testcontainer_remote_ex3 -v /home/user/ex-3:/opt ubuntu_image_remotely';
        }    
	stage ('Format check'){
	    try {
		    sh 'pycodestyle --ignore=E501 python_good.py';
		    println "python_good.py is in good format";
            }catch(x){
                println "python_good.py is in bad format";
                sh 'autopep8 -i python_good.py';
                println "python_good.py is now formatted";
               }
        try {
		    sh 'pycodestyle --ignore=E501 python_bad.py';
		    println "python_bad.py is in good format"
            }catch(y){
                println "python_bad.py is in bad format";
                sh 'autopep8 -i python_bad.py';
                println "python_bad.py is now formatted";
               }
    stage('Pushing the formatted code to BitBucket'){
        dir('/var/lib/jenkins/workspace/Exercise3'){
            try{
                sh 'git config --global user.email "lingojuyogesh.kumar@ltts.com"';
                sh 'git config --global user.name "Yogesh Kumar"'
                sh 'git add python*.py';
                sh 'git commit -m "Commit after auto-format of code"';
                sh 'git push origin master';
		println "Code is pushed successfully.";
                }catch(n){
                    println "Code is already in good format. So, nothing to push."
                }
        }
    }
	}
    }
}
