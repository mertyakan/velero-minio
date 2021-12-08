# velero-minio

1. Bu repoyu clone alın

    ```bash 
    git clone https://github.com/mertyakan/velero-minio
    cd velero
    ```

1. `minio-deploy.yaml` manifest dosyasını deploy edin. Bunun dışında service account, namespace gibi işlemler için klasörü apply edebilirsiniz, Portworx kullanmıyorsanız stateful editleyerek storageclass'ı defaulta çekebilirsiniz,

    ```bash
    kubectl apply ./minio
    ```

1. İlgili podları kontrol edebilirsiniz,

    ```bash
    $ kubectl get pods -n velero
    NAME                      READY   STATUS      RESTARTS   AGE
    minio-0                   1/1     Running     0          3m48s
    minio-setup-zvcdg         0/1     Completed   1          3m47s

    ```

    Dilerseniz port-forwoard gerçekleştirerek localhost:9000 yada LoadBalancer olarak konfigüre edebilirsiniz,

    ```bash
    kubectl port-forward service/minio -n velero 9000
    ```
    
    
## Install Velero with restic

```bash 
velero install                                                                                   \
--provider aws                                                                                   \
--plugins velero/velero-plugin-for-aws:v1.3.0                                                    \
--bucket velero                                                                                  \
--secret-file ./credentials-Velero                                                               \
--use-volume-snapshots=false                                                                     \
--backup-location-config region=minio,s3ForcePathStyle="true",s3Url=http://minio.velero.svc:9000 \
--use-restic
```
    
```bash
$ kubectl logs deployment/velero -n velero
...
time="2020-08-25T15:33:09Z" level=info msg="Server started successfully" logSource="pkg/cmd/server/server.go:881"
time="2020-08-25T15:33:09Z" level=info msg="Starting controller" controller=restic-repository logSource="pkg/controller/generic_controller.go:76"
time="2020-08-25T15:33:09Z" level=info msg="Starting controller" controller=restore logSource="pkg/controller/generic_controller.go:76"
```


    
    
    
    
    
    
    
