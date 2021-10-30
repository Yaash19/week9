podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:jdk8
        env:
        - name: CALCIP
          value: 10.105.216.11
        command:
        - sleep
        args:
        - 99d
        restartPolicy: Never
'''){
 node(POD_LABEL) {
    stage('gradle') {
         git 'https://github.com/Yaash19/week9.git'
        container('gradle') {
        stage('start calculator') {
        sh '''
            echo "start calculator"
             cd Chapter08/sample1
            
            curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments -XPOST -H "Content-type:application/yaml" --data-binary @hazelcast.yaml
            curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments -XPOST -H "Content-type:application/yaml" --data-binary @calculator.yaml
            

            echo "Sum of 3 and 3 is"
            curl -i $CALCIP:8080/sum?a=3\\&b=3


            echo "DIV of 6 and 3 is"
            curl -i $CALCIP:8080/div?a=6\\&b=3


        
        '''
                }
            }
        }
    }
}