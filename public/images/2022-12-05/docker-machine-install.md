Windows makineler için Git BASH indirilir. https://gitforwindows.org/
docker-machine'i indirmek için aşağıdaki komut çalıştırılır.

$ base=https://github.com/docker/machine/releases/download/v0.14.0 &&
  mkdir -p "$HOME/bin" &&
  curl -L $base/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" &&
  chmod +x "$HOME/bin/docker-machine.exe"
  
  ---
  
  ### docker-machine kontrolü yapılır ###
  $ `docker-machine version`
  Çıktı: docker-machine.exe version 0.14.0, build 89b8332
  
  ### Sanal Makina Oluşturma ###
  *&* işareti komutun arkaplanda çalışması için.
  $ `docker-machine create -d=virtualbox --virtualbox-cpu-count=2 --virtualbox-memory=2048 vbox-01 &`
  
  Çıktı : $ ```
  Creating CA: C:\Users\mertt\.docker\machine\certs\ca.pem
Creating client certificate: C:\Users\mertt\.docker\machine\certs\cert.pem
Running pre-create checks...
Error with pre-create check: "This computer is running Hyper-V. VirtualBox won't boot a 64bits VM when Hyper-V is activated. Either use Hyper-V as a driver, or disable the Hyper-V hypervisor. (To skip this check, use --virtualbox-no-vtx-check
  ```
  
 Yukarıdaki şekilde hata alıyorsanız (Hyper-V açıksa bu hata alınır) komutu bu şekilde güncelleyin. `docker-machine create -d=virtualbox --virtualbox-cpu-count=2 --virtualbox-memory=2048 --virtualbox-no-vtx-check vbox-01 &`
 
 
 
 ---
 
 
 ### Docker servis yaratma ###
 Docker servis oluşturma komutu
 `$ docker service create -d --name my_web --limit-cpu=2 --limit-memory 2048mb --replicas 5 --update-delay 10s --update-parallelism 2 -p:8080:80 nginx:latest`
 
 Docker servisinin task'lerini listeleme
 `docker service ps my_web`
 


  