# Zeppelin_and_MSSQL, some steps are extra and specific to restrictions of corporate networks
Setting up Apache Zeppelin on Linux VM and retrieving data from MS SQL
obtain the latest version from the project site https://zeppelin.incubator.apache.org/download.html 
and Microsoft JDBC driver from Microsoft site
copy files to virtual machine

sudo cp /media/sf_Share/zeppelin-0.5.6-incubating-bin-all.tgz .
sudo cp /media/sf_Share/sqljdbc_4.2/enu/sqljdbc* .
sudo chown ims:ims *

expand archive

tar -xzf zeppelin-0.5.6-incubating-bin-all.tgz
cd zeppelin-0.5.6-incubating-bin-all/
cp ../sqljdbc* interpreter/spark/
./bin/zeppelin-daemon.sh start

connect to localhost:8080 from your browser

Create a new note and run following code:
#Please use your own database name and user name/password
%spark println(sc)
println(sqlContext)
val InvLinesDF = sqlContext.load("jdbc", Map( "driver" -> "com.microsoft.sqlserver.jdbc.SQLServerDriver", "url" -> "jdbc:sqlserver://10.100.49.52:1433;database=dbname;user=username;password=userpassword;trustServerCertificate=false;loginTimeout=30;", "dbtable" -> "tbInvoiceLine"))
InvLinesDF.registerTempTable("tInvLines")
sqlContext.sql("select * from tInvLines")
%sql select count(*) from tInvLines
%sql select * from tInvLines
