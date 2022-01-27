# Crud-App_Operator 

> This repo is generated from the Operator-SDK framewok example of memcache.
<USER_REPO>
#### * The App used to deploy in the pods:
Docker Image 🐳 
  >`<USER_REPO>/crud-app:sc-load`

#### * The Operator Image
 > `<USER_REPO>/crudapp-optest:v3`

---
## Commands to genrate and deploy the image
 
#### 1.Accomadate the changes  in the Operator code base.
   ```
    make generate 

    make manifests   
   ```
#### 2.Create the docker Image of the operator. 
   ```
   make docker-build IMG=<USER_REPO>/crudapp-optest:v3

   make docker-push IMG=<USER_REPO>/crudapp-optest:v3
   ```
#### 3.Deploy the operator in the OCP cluster. 
`make deploy  IMG=<USER_REPO>/crudapp-optest:v3`
  
#### 4.Issue a pod against the Operator deployed.
`kubectl apply -f ./config/samples/cache_v1alpha1_memcached.yaml`
   
#### 5.Clean up the Environment created. 
```
  kubectl delete -f config/samples/cache_v1alpha1_memcached.yaml 

  make undeploy IMG=<USER_REPO>/crudapp-optest:v3
```
***

## Images of the Operator in action 

#### - Deployments (1.Operator 2. App Deployment )

>![Not Found ](https://github.com/j4real2208/CRUDapp-operator/blob/main/resources/Deploy.png)

#### - The Config Map 

>![Not Found ](https://github.com/j4real2208/CRUDapp-operator/blob/main/resources/cm.png)

#### - Service

>![Not Found ](https://github.com/j4real2208/CRUDapp-operator/blob/main/resources/svc.png)


#### - Route

>![Not Found ](https://github.com/j4real2208/CRUDapp-operator/blob/main/resources/route.png)



---
## Things to remember

* K8's annotations carry a lot of value.This addition allows clusterroles in OCP cluster to create CR's of the mentioned kinds.
  Added to <api>_controller.go
     ```
       //+kubebuilder:rbac:groups=core,resources=secrets,verbs=get;list;watch;create;update;patch;delete
       //+kubebuilder:rbac:groups=core,resources=services,verbs=get;list;watch;create;update;patch;delete
       //+kubebuilder:rbac:groups=core,resources=configmaps,verbs=get;list;watch;create;update;patch;delete
       //+kubebuilder:rbac:groups=core,resources=configmaps,verbs=get;list;watch;create;update;patch;delete
       // +kubebuilder:rbac:groups=route.openshift.io,resources=routes,verbs=get;list;watch;create;update;patch;delete
       // +kubebuilder:rbac:groups=route.openshift.io,resources=routes/custom-host,verbs=get;list;watch;create;update;patch 
     ```
* Route Schema has to be added seperately since its not of the standard library classes.To be added in main.go 
`utilruntime.Must(routev1.AddToScheme(scheme))`
-  Reference:- https://github.com/operator-framework/operator-sdk/issues/1365
* Volumemounts to be taken care of while writing deployment logic.
