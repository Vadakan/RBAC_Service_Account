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




