---
layout: post
title: Docker Swarm (Container Orchestration Tool) Nasıl Kullanılır?
main_image : {{site.baseurl}}/public/images/2022-12-05/docker-swarm.png

---

<ins>Kurulum Windows10 işletim sisteminde yapılmıştır<ins>

## Kuruluma Hazırlık ##
---

### Git Bash ###
Terminal komutlarını kullanmak için [Git BASH](https://gitforwindows.org/) indirilir.
 

### Docker Machine ###
Local bilgisayarda, bulut sağlayıcılarda ve data center'ların içinde birden fazla host oluşturmayı sağlar.
Sunucular yaratır, onlara Docker yükler ve onların Docker client'larla iletişimini sağlar.

docker-machine'i indirmek için aşağıdaki komut çalıştırılır.

``` bash
$ base=https://github.com/docker/machine/releases/download/v0.14.0 &&
  mkdir -p "$HOME/bin" &&
  curl -L $base/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" &&
  chmod +x "$HOME/bin/docker-machine.exe"

```
  
Docker Machine Kontrolü

`$ docker-machine version`

<span class="result">docker-machine.exe version 0.14.0, build 89b8332</span>

---

## Sanal Makina Oluşturma ##
 Driver olarak virtual-box seçilmiştir. `&` işareti komutun arkaplanda çalışması için. *vbox-01* adında bir sanal makine oluşturalım.

`$ docker-machine create -d=virtualbox --virtualbox-cpu-count=2 --virtualbox-memory=2048 vbox-01 &`

<span class="result">
Creating CA: C:\Users\mertt\.docker\machine\certs\ca.pem
Creating client certificate: C:\Users\mertt\.docker\machine\certs\cert.pem
Running pre-create checks...
Error with pre-create check: "This computer is running Hyper-V. VirtualBox won't boot a 64bits VM when Hyper-V is activated. Either use Hyper-V as a driver, or disable the Hyper-V hypervisor. (To skip this check, use --virtualbox-no-vtx-check</span>

Yukarıdaki şekilde hata alıyorsanız (Hyper-V açıksa bu hata alınır) komutu bu şekilde güncelleyin.

 `$ docker-machine create -d=virtualbox --virtualbox-cpu-count=2 --virtualbox-memory=2048 --virtualbox-no-vtx-check vbox-01 &`

![creating-vm-1-completed]({{site.baseurl}}/public/images/2022-12-05/creating-vm-1-completed.PNG)
*vbox-01* adında bir makine oluştu.

Bunun gibi 2 tane daha sanal makina oluşturalım.(vbox-02, vbox-03)

Docker Machine Listeleme

`$ docker-machine ls`
![docker-machine-list]({{site.baseurl}}/public/images/2022-12-05/docker-machine-list.PNG)

Docker Machine'ın İçin Bazı Gerekli Komutlar
- *Env'lari görme* : `$ docker-machine env vbox-01`
![connect-vm]({{site.baseurl}}/public/images/2022-12-05/connect-vm.PNG)
- *Bir  makineye bağlanma* : `$ eval "$(docker-machine env vbox-01)"`
- *Hangi makinede olduğunu görme* : `$ docker info | grep -i name`
![connect-vm-check]({{site.baseurl}}/public/images/2022-12-05/connect-vm-check.PNG)

---

## Docker Swarm ##
Sanal makinaları 1 Manager ve 2 worker olarak belirtelim. Her bir sanal makina *node* olarak adlandırılır.
*vbox-1*'i lider makine olarak seçelim. Lider *manager'lar* arasından seçilir. Bu örnekte 1 manager olacağı için lider de odur. Lider worker'lara komut verir.

`$ eval "$(docker-machine env vbox-01)"` komutuyla *vbox-01* makinasına geçiş yapalım.

`$ docker swarm init ` komutunu çalıştıralım.
![swarm-init-error]({{site.baseurl}}/public/images/2022-12-05/swarm-init.PNG)

Bu şekilde bir hata ile karşılaşılırsa `$ docker swarm init --advertise-addr 192.168.99.100` komutunu çalıştıralım. IP'ler değişkenlik gösterebilir.

![swarm-init-1]({{site.baseurl}}/public/images/2022-12-05/swarm-init-1.PNG)

Yukarıdaki resimde de görüldüğü üzere bize verilen komutu *worker* makinalarda çalıştıralım.

*vbox-02* worker makinasına geçiş yapalım. 

`$ eval "$(docker-machine env vbox-02)"`

*vbox-02* makinasında olduğumuzu kontrol edelim.

`$ docker info | grep -i name`

 Lider makinadan bize verilen komutu bu makinada çalıştıralım.
![join-to-swarm-as-a-worker-vm-2]({{site.baseurl}}/public/images/2022-12-05/join-to-swarm-as-a-worker-vm-2.PNG)

Böylece *vbox-02* makinası ile iletişim kuruldu.


*vbox-03* worker makinasına geçiş yapalım. 

`$ eval "$(docker-machine env vbox-03)"`
![join-to-swarm-as-a-worker-vm-2]({{site.baseurl}}/public/images/2022-12-05/join-to-swarm-as-a-worker-vm-3.PNG)
vbox-03 makinasında olduğumuzu kontrol edelim.

`$ docker info | grep -i name`

Lider makinadan bize verilen komutu bu makinada çalıştıralım.

![join-to-swarm-as-a-worker-vm-3-1]({{site.baseurl}}/public/images/2022-12-05/join-to-swarm-as-a-worker-vm-3-1.PNG)
 
Böylece *vbox-03* makinası ile iletişim kuruldu.


### Node'ları Listeleme ###
`$ eval "$(docker-machine env vbox-01)"` komutu ile lider makinaya geçelim.
`$ docker node ls` komutu ile makinaların birbirleriyle iletişim halinde olduğu görürüz.

![join-to-swarm-as-a-worker-vm-3-1]({{site.baseurl}}/public/images/2022-12-05/see-node-list.PNG)


### Docker Service ###

Docker service ile işlem yapmak için lider makinaya geçelim.

`$ eval "$(docker-machine env vbox-01)"`

Docker servis yaratma komutu aşağıdaki gibidir.

`$ docker service create -d --name my_web --limit-cpu=2 --limit-memory 2048mb --replicas 5 --update-delay 10s --update-parallelism 2 -p:8080:80 nginx:latest`

Yukarı komut şu anlama gelmektedir;
- `-d` detach (arkaplanda çalış).
- `--name my_web ` servisin adı *my_web* olsun.
- `--limit-cpu=2` en fazla 2 cpu kullan.
- `--limit-memory 2048mb` en fazla 2gb ram kullan.
- `--replicas 5` 5 kopya oluştur.
- `--update-delay 10s` Güncelleme (container silinir ve yeniden oluşturulur) arasında 10sn bekle.
- `--update-parallelism 2` Aynı anda 2 container güncelle (sil ve yeniden yarat).
- `-p:8080:80` Host'un 8080 portuna gelen istekleri container'ın 80 portuna yönlendir.
- `nginx:latest` nginx image'ını kullan.

Bu servis 5 tane task açar ve bu task'ler 5 tane container ayağa kaldırır.

---
Docker servisinin çalışan task'lerini listeleme (Her task bir container oluşturur.)

`$ docker service ps my_web`
![service-task-list]({{site.baseurl}}/public/images/2022-12-05/service-task-list.PNG)


<ins>Artık Docker Swarm' ın çalışıp çalışmadığını deneyebiliriz.<ins>

Bu node'daki container'ları listeyelim.

`$ docker container ls`

![node-container-list]({{site.baseurl}}/public/images/2022-12-05/node-uzerindeki-cont-list.PNG)


667 id'li container'ı kill edelim.

`$ docker container kill 667`

Servislerimizin durumunu lider makinadan gözlemleyelim.

`$ docker service ps my_web`

![service-task-list]({{site.baseurl}}/public/images/2022-12-05/node-uzerindeki-cont-list_kill-and-create_new_cont.PNG)

Yukarıdaki resimde görüldüğü üzere bir container failed durumda, fakat onun yerine başka bir container oluşturdu. 
5 container oluşturulsun emrimiz yerine getirildi.
Böylelikle Docker Swarm ile Container Orchestration işlemimiz başarılı oldu.
---

