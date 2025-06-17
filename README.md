**VPC-PEERING**
VPC ==> (Virtual Private Cloud) VPC gives isolation for your application. Its allow to create public and private networks in it's customizable infrastructure. Actually VPC is used for security purpose, 

  **HOW TO  DO VPC PEERING**

  STEP-1 : **Create VPC**
      -----> open aws console --> search vpc and click on it --> create vpc --> VPC only --> Give a vpc name --> give IPv4 CIDR (10.0.0.0/16) --> click on create vpc.
             Create another vpc with the same process, also check both vpc are creating on same region. (assume you create Test-vpc [10.0.0.0/16] and Dev-vpc [20.0.0.0/16])
  STEP-2 : **Create Subnet**
      -----> On the same page you will get subnet --> click on it --> create subnet --> select the VPC to create the subnet --> give a name of the subnet --> give availability zone --> give 
IPv4 subnet CIDR block (10.0.0.0/24) --> create subnet.
             Create subnet for both vpc (Test-subnet [10.0.0.0/24], Dev-subnet [20.0.0.0/24])
  Step-3 : **Create Internet-Gatewayy**
      -----> When we create a subnet under a VPC, it's actually create a private subnet, If you give the subnet Internet Gateway access then the subnet will become a public subnet Internet-Gateway give access to the outer world.
      On the same page you got Internet-Gateway, click on that --> click on internet-gateway --> Give a name --> create --> after creation click on action --> attach VPC --> select the vpc
       Create 2 Internet-Gateway for the two vpc (Test-IGW, Dev-IGW)
  Step-4 : **Create Route Table**
      -----> Internet-Gateway allow to come internet and to give a route/path to that internet required Route table.
      On the same page go to route table --> create Route Table --> Give a name --> select vpc --> Create route table.
      Create for the both VPC. In my case I created (Test-RT, Dev-RT)
  Step-5 : **subnet-association**
      -----> In my VPC now available Internet-Gateway, Route table. Now incoming internet get the route/path but don't know on which subnet need to reach. For solving this we need to configure subnet-association
       click on Route table --> go to subnet association --> edit subnet association --> check mark on that --> save association.
       Doing this for both subnet
  Step-6 : **Meet Route table and Internet-Gateway**
      -----> Route table know on which subnet need to go but Internet-Gateway don't know who is route table, so its time to meet Internet-Gateway and Route Table.
      click on the subnet --> go to route --> edit route --> add route --> destination 0.0.0.0/0 --> target: Internet-Gateway --> select the correct IGW --> save changes.
      Doing this for both subne.
 Step-7 : **Create server on VPC**
     -----> open aws interface --> search EC2 --> and click on that --> give a name of the EC2 --> give a os image (ubuntu) --> select instance type (t2.micro) --> select a key or create a key --> search network setting and click on edit --> select your vpc and subnet --> enable auto assign IP --> give a security rule for HTTP and give the source of your vpc CIDR (for Test-vpc 10.0.0.0/0) --> select your storage and click on launch instance.
     Create two server for two vpc
Step-8 :  **Peering connection**
    -----> Open the vpc page on aws console --> go to peering connection --> create peering connection --> give a name of the peering connection setting --> give your first vpc name --> account: My account --> Region: This region (Must check that both VPC are created in same region) --Create peering connection 
Step-9 : **Give both server security access for the both VPC to connect each other**
    -----> Open aws console and click on EC2 --> select a server --> go to security -- click on edit inbound rule --> add inbound rule --> type: custom ICMP IPv4 , CIDR 10.0.0.0/16 for Test and 20.0.0.0/16 for Dev --> click on save rules.
    Give the same access for both server.
Step-10 : **Give route access for each othe**
    -----> Open aws console --> search route table --> select route --> edit routes --> add routes --> destination (For Test [20.0.0.0/16], for Dev [10.0.0.0/16]) --> target: peering connection, select the connection click on save changes.
      Doing this for both route table (Test-RT, Dev-RT)

  Step-11 : **Finally check Result **
      -----> Select instances one by one and click on connect --> select EC2 instance connect and click on connect. --> now after connecting the server took Dev server private Ip and go to Test server and run this command : ping server privatr Ip --> click on enter and do the vice-versa. 
