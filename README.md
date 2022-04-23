# RBAC_Service_Account

# below all concepts are mainly used to secure the cluster. How we can secure our cluster for the users.

# To understand the concept of 'service account' we have to first understand the Role Based Access Control (RBAC).

# Kube-api-server is the component in kubernetes master which takes the user's API request. This component used 3 step mechanism to allow the user to 
# perform actions in the kubernetes cluster

# 1) Authentication 2) Authorization 3) admission control

# RBAC


![image](https://user-images.githubusercontent.com/80065996/164909680-e906b302-757d-402b-aa79-719da117c856.png)



![image](https://user-images.githubusercontent.com/80065996/164909717-894a83c3-02e0-4b5e-8906-b6e80b2389f6.png)


![image](https://user-images.githubusercontent.com/80065996/164909746-f77666e5-a17d-4978-a3ac-1b562e04cc26.png)


![image](https://user-images.githubusercontent.com/80065996/164909796-99aad9cb-b919-4664-9084-5c8ff7c44375.png)


# NOTE: AUTHENTICATION IS NOT INBUILT IN KUBERNETES. WE HAVE TO REPLY ON THIRD PARTY AUTHENTICATOR LIKE AZURE AD (AZURE ACTIVE DIRECTORY)


![image](https://user-images.githubusercontent.com/80065996/164910057-9893aa6f-125c-4d91-bd17-a0abc19ad3bc.png)


# HOW RBAC works ?

# Scenario : suppose a new person joined in the team. His name is 'Ram'. He joins in a development team. Now kubernetes admin has a task to assign the 
# access to that person 'ram' into kubernetes cluster. Kubernetes cluster have many 'namespaces'. for example, one namespace for dev team and other namespace for QA (testing) team. since 'ram' is from developer team kubernetes admin has to provide access to 'ram' only to developer namespace. 'ram' should not be provided access to QA namespace. so next step will be creating 'certificate credentials' for 'ram' to get authenticated into kubernetes cluster. so after generating certificate credentials 'ram' will get authenticated to enter into the cluster.  After authenticated and entering into kuberneted cluster, we should restrict 'ram' to access only speicified resources which needed for development team. kubernetes admin will achieve this using concept called authorization. so now 'ram' has to get a 'role' and 'role binding' to access only specified resources


![image](https://user-images.githubusercontent.com/80065996/164910303-030d3675-ae7d-4886-b31e-f3ae27c613db.png)


![image](https://user-images.githubusercontent.com/80065996/164910850-4a6cad0f-8207-4ba4-979b-559caba145af.png)


# Two terms we have to remember always. 1) who  and 2) what

# who ? is nothing but 'user' or 'group of users' or a 'process' which can trigger the event.
# for example: in case of 'auto scaling' scenario, a 'user' can type a 'kubectl scale' command to scale the pods. or a HPA (horizontal Pod auto scaler) which in turn trigger the event of scaling. so when a process like HPA (auto scaler) done this, it is called as 'service account' .

# Either i can be a user or either i can be a user belong to group whether manager group or tester's group, i can attach a role to that group, either testes's group or manager's group so that individually i cannot assign the role to the users. 
# In case of service account, it will be attached to particular resources, if a role is attached to service account, whoever the users attached to the role, will perform actions on the resources.

![image](https://user-images.githubusercontent.com/80065996/164911135-4afedc54-b51c-4803-9230-fb9afc2fa9eb.png)


![image](https://user-images.githubusercontent.com/80065996/164911331-33c1f63f-c56a-464f-8396-a5a1031c5fe2.png)


![image](https://user-images.githubusercontent.com/80065996/164911591-86cc39a7-11c3-41ca-91a0-941dd11ea6e4.png)


# Note: 'Role' and 'Role binding' works only in the single namespace level.


# 'CLUSTER ROLE' AND 'ROLE BINDING' ===> WORKS IN CLUSTER LEVEL AND APPLICABLE TO ALL THE NAMESPACES.


![image](https://user-images.githubusercontent.com/80065996/164911857-cc86c57f-0dfd-4037-afc0-10adc4cf6629.png)


# Demo of service account concepts:

# get the service account available currently in the namsepace


![image](https://user-images.githubusercontent.com/80065996/164915149-efd03ca9-e4d5-409c-a8eb-af413b8e1e52.png)


# create the new 'service account' named 'aspa1' in the 'msundarmunichamy-dev' namespace.
# describe the 'service account' 'aspa1' created using 'kubectl describe' command 


![image](https://user-images.githubusercontent.com/80065996/164915827-f7cdb3e0-97ae-4a2b-8ba5-dab4188a2f8f.png)


# if we create 'service account', many 'secrets' will be created by default with the prefix name of 'service accounts'
# trying to get the details of one of the 'secret' created called 'mountable secret' as highlughted below.


![image](https://user-images.githubusercontent.com/80065996/164915925-71c3489c-4a10-44e9-bc4b-bdd030418eb1.png)


# try to get the yaml details of the 'secrets' created 


![image](https://user-images.githubusercontent.com/80065996/164916065-edf1e18a-acd1-4576-8078-18a55a061d99.png)


# Look for the 3 main things in the yaml. (meaning 3 important fields)

# 1) ca.crt
# 2) namespace
# 3) token

# depending on the type of secret, some more fields will also be there. ( you can see one secret created for 'image pull' and  some other secrets for 'tokens' and some other secrets for 'mountable secrets'


![image](https://user-images.githubusercontent.com/80065996/164916273-ee677f01-d777-41ce-98f2-9158a779aa91.png)


![image](https://user-images.githubusercontent.com/80065996/164916289-f25a4f70-eb2c-41b7-a3f0-61319a2d4605.png)


![image](https://user-images.githubusercontent.com/80065996/164916303-f6b46dec-22e8-482a-b96d-c4fb82234b0e.png)


# we can take the 'namespace' details and decode it like below to see actual content of it


![image](https://user-images.githubusercontent.com/80065996/164916504-c00a7edd-ad61-40b8-8637-76178dc31919.png)


# Take the ca.crt content from the 'secret's' yaml file


![image](https://user-images.githubusercontent.com/80065996/164924715-c8b9337d-0c92-4cc8-b4bb-b2ecc9245cd6.png)


# we are going to check the same ca.crt is present in 'kube-api' server. because if service account needs to get authenticated, request has to be made to the 'kube-api-server' which runs on the kuberneted master. so we are going to check that

# taking the pod details of 'kube-api-server'


![image](https://user-images.githubusercontent.com/80065996/164921726-452b0ed9-e825-4ed9-9be4-49cb2f148cbc.png)


![image](https://user-images.githubusercontent.com/80065996/164928533-e35af321-b935-4c9b-b4a6-854130f98bf7.png)

# edit the pod using 'kubectl edit' and identify the location of 'cert file' present


![image](https://user-images.githubusercontent.com/80065996/164929560-ce3e1284-fbb9-4718-83ec-7949a7d24da1.png)




