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
			def success=0
			echo logCust
			if (logCust == success ) {
			
					echo " No Error log generated for script Load Customer"
					}
			else
					throw err				
			}
		catch(err){
			echo "Error exists in  Git load_data_customer Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }
        
        }
       stage ('Loading Account Data') {	
	   try{
			bat "load_account.bat"
			def logCust = readFile "${env.WORKSPACE}/load_account_error.txt"
			if(logCust == '0')
					echo " No Error log generated for script Load Account"
			else
					throw err				
			}
		catch(err){
			echo "Error exists in  Git load_data_account Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }          
        }
        stage ('Loading Transaction Data') {
		  try{
			bat "load_transaction.bat"
			def logCust = readFile "${env.WORKSPACE}/load_transaction_error.txt"
		if(logCust == '0')
					echo " No Error log generated for script Load Account"
		else
					throw err				
			}
		catch(err){
			echo "Error exists in  Git load_data_account Script, Marking build as unstable"
			currentBuild.result = "UNSTABLE"
					   }
}					   
        
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}
