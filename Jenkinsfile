import jenkins.model.*
import hudson.model.*
import java.text.SimpleDateFormat 
import java.util.*
import java.util.Date

node(){
/** will give all the jobs present in jenkins **/
List successListJob = new ArrayList();
List successListRunningTime= new ArrayList();
List successListLastRunTime= new ArrayList();

List failListJob= new ArrayList();
List failListRunningTime= new ArrayList();
List failListLastRunTime= new ArrayList();

stage("Start"){
    def item
def ifloop
def elseifloop
long currentTime=System.currentTimeMillis()
//print "Current Time "+currentTime
long lastJobRunTime 
long minsCon = 30*60000;
long diffTime =currentTime-minsCon;
SimpleDateFormat date = new SimpleDateFormat("EEE MMM dd HH:mm:ss z yyyy")
//println "different time "+diffTime
List list =Jenkins.instance.getAllItems(AbstractItem.class)

start = System.currentTimeMillis()  
def jobsFound = []




for(int i=0; i<list.size();i++){
  //println "***********START LOOP *********"+list.get(i).fullName   
       try{
	       /** for the folder view jobs **/
           if(list.get(i).fullName.contains('/')){
                item= Jenkins.instance.getItemByFullName(list.get(i).fullName)
           }else{
		       /**  Normal jobs **/
               item = Jenkins.instance.getItem(list.get(i).fullName)    
           }
		   /** if last build and last successfull build same then the job is success **/
        if(item.getLastBuild()==item.getLastSuccessfulBuild() && !(item.getLastBuild()==null)){
         // print("The job "+list.get(i).fullName+"  SUCCESS!!!!!!!!!!")
            ifloop=item.getLastSuccessfulBuild()
            lastJobRunTime =date.parse(ifloop.getTime().toString()).getTime()
         //   print "Last Successful job run time "+lastJobRunTime
		    /** checks the success job run time is more than 30mins different and less than the current time then will give the how much time it took to complete**/
            if(lastJobRunTime>diffTime && lastJobRunTime<currentTime){
              action = ifloop.getAction(jenkins.metrics.impl.TimeInQueueAction.class)
             //  println ("Success "+list.get(i).fullName+" Time in Mins  "+action.getTotalDurationMillis()/(1000*60) +" and last run time is "+elseifloop.getTime());
              successListJob.add(list.get(i).fullName.toString())
              successListRunningTime.add(action.getTotalDurationMillis()/(1000*60) )
              successListLastRunTime.add(ifloop.getTime())
              
             // println ("Success job size "+successListJob.size()+ " successListRunningTime "+successListRunningTime.size()+" successListLastRunTime "+successListLastRunTime.size())
             
            }
        }else if(item.getLastBuild()==item.getLastFailedBuild() && !(item.getLastBuild()==null)){
              elseifloop=item.getLastFailedBuild()
              lastJobRunTime =date.parse(elseifloop.getTime().toString()).getTime()
            /** checks the fail job run time is more than 30mins different and less than the current time then will give the how much time it took to complete**/
            if(lastJobRunTime>diffTime && lastJobRunTime<currentTime){
              action1 = elseifloop.getAction(jenkins.metrics.impl.TimeInQueueAction.class)
              //println ("Failed "+list.get(i).fullName +" Time in Mins "+action1.getTotalDurationMillis()/(60*1000) +" and last run Run time is "+elseifloop.getTime());
              failListJob.add(list.get(i).fullName.toString())
              failListRunningTime.add(action1.getTotalDurationMillis()/(1000*60) )
              failListLastRunTime.add(elseifloop.getTime())
            //  println ("Failed job size "+successListJob.size()+" and "+failListRunningTime.size())
              
            }
        }//else
          //  print "The job "+list.get(i).fullName+ " didn't run at all "
           
       }catch(Exception e ){
           
       //println "*******CATCH*************=="+list.get(i).fullName
       } 
}

}
stage('Failed')
{
    print "Failed"
    println "Failed job list size is "+failListJob.size()
    for(int i=0;i<failListJob.size();i++){
    println ("Failed "+failListJob.get(i)+" took Time in Mins "+failListRunningTime.get(i)+ " Last ran time "+failListLastRunTime.get(i))
}
}

stage('passed'){
    print "Success"
    println "Passed job list size is "+successListJob.size()
    for(int j=0;j<successListJob.size();j++){
    println ("Success "+successListJob.get(j)+" took Time in Mins "+successListRunningTime.get(j)+ " Last ran time "+successListLastRunTime.get(j))
}

}
}

