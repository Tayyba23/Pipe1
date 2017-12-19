node {
    // Clean workspace before doing anything
    try {
	   stage ('Clone') {
            checkout scm
            
        }
        stage ('Loading Customer Data') {
	   try {        
			
		
			bat "tbuild -f load_Data_customer.tpt > loadDataCustomerStage_Log.txt"
			echo %ERRORLEVEL%
			}
		catch(err){
			echo "Error exists in  Git load_Data_Customer Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }
        
        }
       stage ('Tests') {	
	  
            
        }
        stage ('Deploy') {
            //update dashboard
             bat "echo 'will update dashbaord...'"
			 echo "test1"
        }
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}
