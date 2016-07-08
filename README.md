
This repo demonstrates openshift v3 s2i scripts usage

> In order to use openshift s2i image  create directory  .s2i and files under s2i as follows   
> To Install s2i command line tool https://github.com/debianmaster/Notes/wiki/Source-2-Image---(s2i)--on-openshift    


#### assemble.sh  
> This file is used dynamically adding artifacts into base image and creating a app image as a result.   

```sh
cp -Rf /tmp/src/. $CATALINA_HOME/webapps
echo "WAR's copied"
```

#### run.sh
> This file is used for mentioning startup script.   

```sh
${CATALINA_HOME}/bin/catalina.sh run
```

### Create builder image
> tomcat8-jdk8  is my future builder image name   

```sh
docker build -t tomcat8-jdk8 .
```

### Test builder image by deploying war
>  on base image tomcat8-jdk8 deploy the war (contents) that is present in test/test-app and make a app image called (tomcat8-jdk8-app)   

```sh
s2i build test/test-app tomcat8-jdk8 tomcat8-jdk8-app
```

### Test the app image
```sh
docker run -d  -p 8080:8080  tomcat8-jdk8-app 
```

## Using the template provided in this repo.
```sh
oc new-build --strategy=docker --name=tomcat8-jdk8 https://github.com/debianmaster/openshift-s2i-example.git -n openshift
oc create -f tomcat8-jdk8-war.template
```
> Goto ui -> Add to project and look for tomcat8-jdk8 template. 
> In the step above we are creating a base image with tomcat and java and storing it on openshift namespace   



## Creating template on Openshift 

> i have arrived at the template  tomcat8-jdk8-war.template in this repo by creating following objects indivudually and then creating a template out of it.  (with few modifications)   

#### Create Image stream & export templates 
```sh
oc new-app tomcat8-jdk8~https://github.com/debianmaster/sample-binaries.git --name='tomcat8-jdk8-war'
oc export is,bc,dc,svc --as-template=tomcat8-jdk8  > template
```

> Modify the template above and rename it as tomcat8-jdk8-war.template   




