node {
    // Clean workspace before doing anything
    try {
	   stage ('Clone') {
            checkout scm      
			bat "bteq <clearTables.txt>  table_clear_Log.txt"
             }
        stage ('Loading Customer Data') {
	   try {        
				
			bat "load_customer.bat"
			def logCust = readFile "${env.WORKSPACE}/load_customer_error.txt"
			echo logCust
			if (logCust.contains('0')) {
			
					echo " No Error log generated for script Load Customer"
					}
			else
					throw err				
			}
		catch(err){
			echo "Error log generated for load_data_customer Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }
        
        }
       stage ('Loading Accounts Data from first file') {	
	   try{
			bat "load_account.bat"
			def logCust = readFile "${env.WORKSPACE}/load_account_error.txt"
			if(logCust.contains('0'))
					echo " No Error log generated for script Load Account Source 1"
			else
					throw err				
			}
		catch(err){
			echo "Error log generated for load_data_account Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }          
        }
		
		   stage ('Loading Accounts Data from second file') {	
	   try{
			bat "load_account_second.bat"
			def logCust = readFile "${env.WORKSPACE}/load_account_2_error.txt"
			if(logCust.contains('0'))
					echo " No Error log generated for script Load Account Source 2"
			else
					throw err				
			}
		catch(err){
			echo "Error log generated for load_data_account Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }          
        }
		
        stage ('Loading Transaction Data') {
		  try{
			bat "load_transaction.bat"
			def logCust = readFile "${env.WORKSPACE}/load_transaction_error.txt"
		if(logCust.contains('0'))
					echo " No Error log generated for script Load Account"
		else
					throw err				
			}
		catch(err){
			echo "Error log generated for load_data_account Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }
}	

        stage ('Running TAF for Row Count Verification') {
				  
		  def status = ""
            try{
              bat "java -jar target\\tafd.jar"
			  def out= "$JENKINS_HOME/jobs/$JOB_NAME/builds/${BUILD_NUMBER}"
				bat "cd $JENKINS_HOME/jobs/$JOB_NAME/builds/${BUILD_NUMBER} \n dir /b /a-d > tmp.txt"
				def files = readFile "$JENKINS_HOME/jobs/$JOB_NAME/builds/${BUILD_NUMBER}/tmp.txt"
				def temp="tmp.txt";
				bat "java -jar LogParser.jar $out temp.txt"
				status = readFile "$JENKINS_HOME/jobs/$JOB_NAME/builds/${BUILD_NUMBER}/result.txt"
				if(status.contains('Unsuccessful')){
					echo status
					throw err 
					}
					}
					catch(err)
					{
					currentBuild.result='UNSTABLE'
					}
		}				   
        
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}
