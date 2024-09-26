# CKA: Certified Kubernetes Administrator Notları
## Index
- [CKA: Certified Kubernetes Administrator Notları](#cka-certified-kubernetes-administrator-notları)
  - [Index](#index)
  - [Bookmarks](#bookmarks)
  - [Cluster Architecture](#cluster-architecture)
  - [Docker ve containerd](#docker-ve-containerd)
  - [CRI, OCI, ImageSpec, RuntimeSpec, rkt, nerdctl, ctl, crictl](#cri-oci-imagespec-runtimespec-rkt-nerdctl-ctl-crictl)
  - [etcd ve etcdctl](#etcd-ve-etcdctl)
  - [Kube API Server](#kube-api-server)
  - [Kube Controller Manager](#kube-controller-manager)
  - [Node Controller](#node-controller)
  - [Replication Controller](#replication-controller)
  - [ReplicaSet](#replicaset)
      - [ReplicaSet Tanımı İçindeki Alanlar ve Olası Değerler](#replicaset-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
      - [Örnek ReplicaSet Yapılandırması ve Alan Seçenekleri](#örnek-replicaset-yapılandırması-ve-alan-seçenekleri)
  - [Kube-Scheduler](#kube-scheduler)
  - [Kubelet](#kubelet)
  - [Kube-proxy](#kube-proxy)
  - [Kubeadm](#kubeadm)
  - [Pod](#pod)
  - [YAML'in Kubernetes ile İlişkisi](#yamlin-kubernetes-ile-i̇lişkisi)
  - [Pod Definition YAML Örnekleri ve Olası Alan Seçenekleri](#pod-definition-yaml-örnekleri-ve-olası-alan-seçenekleri)
      - [Örnek Pod Tanımı (YAML)](#örnek-pod-tanımı-yaml)
      - [Pod Tanımı İçindeki Alanlar ve Olası Seçenekler](#pod-tanımı-i̇çindeki-alanlar-ve-olası-seçenekler)
      - [`containers` Alanındaki Olası Seçenekler](#containers-alanındaki-olası-seçenekler)
  - [Labels ve Selectors](#labels-ve-selectors)
      - [Selectors için Örnek Kullanım Alanları](#selectors-için-örnek-kullanım-alanları)
  - [Deployment](#deployment)
      - [Deployment Tanımı İçindeki Alanlar ve Olası Değerler](#deployment-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
  - [Sertifikasyon İpucu](#sertifikasyon-i̇pucu)
  - [Kubernetes Services](#kubernetes-services)
      - [Service Tanımı İçindeki Alanlar ve Olası Değerler](#service-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
  - [Kubernetes Namespaces](#kubernetes-namespaces)
      - [Namespace Tanımı İçindeki Alanlar ve Olası Değerler](#namespace-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
  - [Resource Quotas (Kaynak Kotaları)](#resource-quotas-kaynak-kotaları)
  - [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
        - [Özet](#özet)
      - [POD](#pod-1)
      - [Deployment](#deployment-1)
      - [Service](#service)
  - [`kubectl apply` Komutu](#kubectl-apply-komutu)
  - [Manuel Scheduling (Manuel Planlama)](#manuel-scheduling-manuel-planlama)
      - [Açıklama:](#açıklama)
  - [Taints ve Tolerations](#taints-ve-tolerations)
      - [Açıklama:](#açıklama-1)
      - [Açıklama:](#açıklama-2)
      - [A quick note on editing Pods and Deployments](#a-quick-note-on-editing-pods-and-deployments)
        - [Edit a POD](#edit-a-pod)
        - [Edit Deployments](#edit-deployments)
  - [Resource Requests ve Limits](#resource-requests-ve-limits)
  - [DaemonSets](#daemonsets)
      - [Açıklama:](#açıklama-3)
  - [Static Pods](#static-pods)
      - [Açıklama:](#açıklama-4)

## Bookmarks

- [Udemy Course](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests)
- [https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/)
- [https://www.cncf.io/training/certification/cka/](https://www.cncf.io/training/certification/cka/)
- [Exam Tips](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
- [Linux Foundation Certification Exam: Candidate Handbook (using PSI BRIDGE Proctoring platform)](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2)
- <https://github.com/cncf/curriculum>
- [Frequently Asked Questions: CKA and CKAD & CKS](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks)
- [https://kodekloud.com/pages/community](https://kodekloud.com/pages/community)
- <https://github.com/mmumshad/kubernetes-the-hard-way>
- <https://github.com/kodekloudhub/certified-kubernetes-administrator-course>
- [https://kubernetes.io/docs/reference/kubectl/conventions/](https://kubernetes.io/docs/reference/kubectl/conventions/)
- https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-scheduling/scheduling_code_hierarchy_overview.md

 > [!IMPORTANT]  
> 28/08/2024 $395 Exam only! +1 free attempt in 12 months. Online 2 hours

## Cluster Architecture

1. **Cluster Components:**
    - **Master Node (Control Plane):** Cluster yönetiminden sorumlu. Genellikle API server, Scheduler, Controller Manager, ve etcd içerir.
    - **Worker Nodes:** Uygulamaların çalıştığı yer. Kubelet, Kube-proxy, ve Container Runtime çalışır.
2. **Control Plane Components:**
    - **API Server:** Tüm kontrol düzlemi bileşenleriyle iletişimi sağlar. `kubectl` komutları buradan geçer.
    - **etcd:** Dağıtık bir key-value store, tüm cluster verilerini saklar. **Kritik:** Yedeklenmeli.
    - **Scheduler:** Pod’ların uygun node'lara atanmasından sorumlu.
    - **Controller Manager:** Farklı kontrol döngülerini çalıştırır (örn. Node Controller, Replication Controller).
3. **Worker Node Components:**
    - **Kubelet:** Pod’ların node’da çalışmasını sağlar, API Server ile iletişim kurar.
    - **Kube-proxy:** Network kurallarını uygular, servisler arası trafiği yönlendirir.
    - **Container Runtime:** Container’ları çalıştırır (örn. Docker, containerd).
4. **Cluster Networking:**
    - **Pod Network:** Tüm Pod’lar arasında iletişimi sağlar. Her Pod, kendi IP adresine sahiptir.
    - **Service Network:** Servisler, belirli bir dizi Pod’a sabit bir IP adresi sağlar.
5. **High Availability:**
    - **Control Plane:** Çoklu master node ile sağlanır. API Server, etcd yedeklenir.
    - **etcd:** Cluster durumunu koruyan kritik bileşen, yedekleme zorunlu.
6. **Add-ons:**
    - **CoreDNS:** DNS hizmeti sağlar, cluster içi ad çözümlemeleri yapar.
    - **Metrics Server:** Cluster performans izleme ve otomatik ölçekleme için kullanılır.
7. **Networking Plugins (CNI):**
    - **Flannel, Calico, Weave:** Pod’lar arasında network kurar ve yönetir.

## Docker ve containerd

1. **Docker ve containerd Nedir?**
    - **Docker:** Konteyner uygulamalarını geliştirmek, dağıtmak ve çalıştırmak için kullanılan popüler bir platformdur. Docker, konteynerleri yönetmek için bir üst düzey API ve kullanıcı dostu araçlar sunar.
    - **containerd:** Docker’ın temelinde yer alan, konteynerlerin çalıştırılması ve yönetilmesi için kullanılan düşük seviyeli bir konteyner runtime’ıdır. Aslen Docker tarafından geliştirilmiştir, ancak artık bağımsız bir proje olarak da kullanılmaktadır.
2. **Docker’ın Bileşenleri:**
    - **Docker Daemon (dockerd):** Konteynerlerin yönetimi ve Docker CLI ile Docker API taleplerinin işlenmesinden sorumlu.
    - **Docker CLI:** Kullanıcıların Docker Daemon ile etkileşime geçmesini sağlayan komut satırı aracıdır.
    - **Docker Engine:** Docker CLI, Docker Daemon ve containerd gibi bileşenlerin tümünü içerir. Konteynerleri oluşturmak, çalıştırmak ve yönetmek için kullanılan tam kapsamlı platformdur.
3. **containerd Özellikleri:**
    - **Lightweight Runtime:** Konteynerlerin yaşam döngüsünü (oluşturma, çalıştırma, durdurma) yönetir, ancak Docker gibi üst düzey özellikler sağlamaz.
    - **Modüler Mimari:** containerd, konteyner snapshot yönetimi, image management, ve network management gibi modüller içerir. Ancak, Docker’ın sunduğu daha geniş ekosistem özelliklerini içermez.
    - **Kubernetes Entegrasyonu:** Kubernetes, containerd’yi native olarak destekler, bu da onu Docker’a alternatif bir runtime olarak kullanmayı mümkün kılar.
4. **Docker ve containerd Arasındaki Farklar:**
    - **Üst Düzey API vs. Low-level Runtime:** Docker, kullanıcı dostu bir API ve CLI ile geniş bir ekosistem sunarken, containerd daha minimal bir runtime olarak işlev görür.
    - **Kapsam:** Docker, containerd üzerinde bir üst katman olarak çalışır ve daha fazla özellik (örn. Docker Compose, Swarm) sağlar. containerd ise sadece konteynerleri çalıştırma ve yönetme işini üstlenir.
    - **Kubernetes Desteği:** containerd, Kubernetes tarafından daha doğrudan bir şekilde desteklenir. Docker, Kubernetes ile kullanıldığında ek bir katman olarak çalışır (dockershim). Ancak, Kubernetes 1.20 sürümünden itibaren dockershim desteği kaldırıldı ve containerd gibi native runtime’lar önerilmeye başlandı.
5. **Kullanım Senaryoları:**
    - **Docker:** Geliştirme ve küçük ölçekli projeler için, Docker'ın sunduğu kullanıcı dostu araçlar ve kapsamlı özellikler nedeniyle tercih edilir.
    - **containerd:** Büyük ölçekli ve üretim ortamlarında, Kubernetes ile birlikte daha hafif bir runtime olarak containerd tercih edilir. Daha az overhead ve doğrudan Kubernetes entegrasyonu sunar.

## CRI, OCI, ImageSpec, RuntimeSpec, rkt, nerdctl, ctl, crictl

1. **CRI (Container Runtime Interface):**
    - **Tanım:** Kubernetes’in farklı konteyner runtime’ları ile etkileşim kurmasını sağlayan bir arayüzdür. CRI, Kubernetes’in bağımsız bir platform olarak çalışmasını sağlar ve farklı runtime’ların Kubernetes’e entegre edilmesini kolaylaştırır.
    - **Kullanım:** Kubernetes, konteyner runtime’ı ile iletişim kurmak için CRI’yi kullanır. Bu sayede Docker, containerd gibi runtime’larla sorunsuz çalışabilir.
2. **OCI (Open Container Initiative):**
    - **Tanım:** Konteynerlerin çalıştırılması, dağıtılması ve taşınabilirliği için standartlar geliştiren bir projedir. OCI, konteyner ekosisteminde uyumluluk ve birlikte çalışabilirliği teşvik eder.
    - **Önemli Standartlar:**
        - **ImageSpec:** Konteyner imajlarının nasıl oluşturulacağını ve saklanacağını tanımlar.
        - **RuntimeSpec:** Konteynerlerin nasıl çalıştırılacağını ve yaşam döngülerinin nasıl yönetileceğini tanımlar.
    - **Bağlam:** OCI’nin sunduğu standartlar, farklı konteyner runtime’larının ve araçlarının birbiriyle uyumlu olmasını sağlar. CRI gibi arayüzler, OCI standartlarına dayanarak çeşitli runtime’lar arasında uyum sağlar.
3. **ImageSpec:**
    - **Tanım:** Konteyner imajlarının formatını tanımlar. Bu standart, imajların nasıl paketleneceğini, dağıtılacağını ve çalıştırılacağını belirtir.
    - **Bağlam:** Docker ve containerd gibi araçlar, imajları bu standarda göre oluşturur ve dağıtır. Bu sayede farklı runtime’lar aynı imajları çalıştırabilir.
4. **RuntimeSpec:**
    - **Tanım:** Konteynerlerin çalışma zamanında nasıl davranacağını ve yönetileceğini tanımlar. Bu standart, konteynerlerin oluşturulması, başlatılması, durdurulması gibi işlemleri içerir.
    - **Bağlam:** Containerd, rkt gibi runtime’lar, OCI RuntimeSpec’e göre çalışır, bu da farklı platformlar arasında tutarlılık sağlar.
5. **rkt (Rocket):**
    - **Tanım:** CoreOS tarafından geliştirilen, güvenlik ve basitlik odaklı bir konteyner runtime’ıdır. Kubernetes’in early support listesinde yer alıyordu, ancak Docker ve containerd kadar yaygınlaşmadı.
    - **Bağlam:** Rkt, Docker’a bir alternatif olarak geliştirilmişti, ancak OCI standartlarının yaygınlaşmasıyla birlikte kullanım alanı daraldı.
6. **nerdctl:**
    - **Tanım:** Docker CLI'ya benzer bir komut satırı aracı olup, containerd ile doğrudan çalışır. Docker’ın CLI deneyimini containerd kullanıcılarına sunar.
    - **Bağlam:** Nerdctl, containerd kullanarak konteynerleri yönetmek isteyenler için hafif ve Docker benzeri bir araçtır. OCI ImageSpec ve RuntimeSpec’e uygun çalışır.
7. **ctl (Control Tools):**
    - **Tanım:** Farklı CLI araçlarının genelleştirilmiş adıdır. Örneğin, kubectl, nerdctl, crictl gibi araçlar konteyner ve Kubernetes yönetimi için kullanılır.
    - **Bağlam:** Her bir ctl aracı, belirli bir platform veya runtime için yönetim ve kontrol işlevleri sağlar. Bu araçlar, kullanıcıların farklı ortamları yönetmesini kolaylaştırır.
8. **crictl:**
    - **Tanım:** CRI uyumlu konteyner runtime’larını yönetmek için kullanılan bir CLI aracıdır. CRI üzerinden çalışan runtime’larla doğrudan iletişim kurabilir.
    - **Bağlam:** Crictl, Kubernetes’in farklı runtime’larla çalışmasını sağlamak için kullanılan bir araçtır. Containerd gibi CRI uyumlu runtime’lar üzerinde operasyonel işler yapılmasını kolaylaştırır.

## etcd ve etcdctl

1. **etcd Nedir?**
    - **Tanım:** etcd, yüksek erişilebilirlik ve tutarlılık sağlayan bir dağıtık key-value store’dur. Kubernetes gibi sistemlerde cluster durumu, yapılandırma verileri, servis keşfi gibi kritik verilerin saklanması için kullanılır.
    - **Kullanım:** Kubernetes’te etcd, tüm cluster durumunu (Pod, Service, ConfigMap, Secret vb.) saklar. Bu veriler, etcd sayesinde hızlıca erişilebilir ve cluster’daki tüm bileşenler arasında senkronize kalır.
    - **Özellikler:**
        - **Güvenilirlik:** Dağıtık yapısı sayesinde veri kaybı riskini minimize eder.
        - **Consistency (Tutarlılık):** Aynı anda birden fazla işlem yapılmasına izin vermez, böylece veriler tutarlı kalır.
        - **Leader Election:** Cluster içindeki nodelar arasında lider seçimi yaparak verilerin güncellenmesini ve senkronize kalmasını sağlar.
        - **Snapshot ve Backup:** etcd verileri, snapshot alınarak yedeklenebilir ve geri yüklenebilir.
2. **etcdctl Nedir?**
    - **Tanım:** etcd veritabanını yönetmek için kullanılan komut satırı aracıdır. etcd üzerinden veri yazma, okuma, silme, yedekleme gibi işlemler yapmanıza olanak tanır.
    - **Kullanım:**
        - **Veri Yönetimi:** `put`, `get`, `del` gibi komutlarla etcd’deki veriler yönetilebilir.
        - **Cluster Yönetimi:** `member add`, `member remove`, `member list` gibi komutlarla etcd cluster’ı yönetilebilir.
        - **Yedekleme:** `snapshot save` komutu ile etcd verilerinin yedeği alınabilir, `snapshot restore` ile geri yüklenebilir.
    - **Örnek Komutlar:**
        - **Veri Ekleme:** `etcdctl put <key> <value>`
        - **Veri Okuma:** `etcdctl get <key>`
        - **Snapshot Alma:** `etcdctl snapshot save <file_path>`
        - **Snapshot Yükleme:** `etcdctl snapshot restore <file_path>`
    - **V3 API:** etcdctl v3, etcd’nin API v3 sürümü ile uyumlu olarak çalışır ve bu sürümde transaction, leasing gibi gelişmiş özellikler sunar.
3. **etcd Kullanımında Dikkat Edilmesi Gerekenler:**
    - **Yedekleme:** etcd veritabanı, Kubernetes için kritik olduğundan düzenli olarak snapshot alınmalıdır.
    - **Güvenlik:** etcd’ye erişim sadece yetkili kişiler tarafından yapılmalıdır. Bu yüzden, etcd veritabanı şifrelenmeli ve TLS ile korunmalıdır.
    - **Yüksek Erişilebilirlik:** etcd cluster, genellikle 3, 5 veya 7 node’dan oluşur. Tek sayıda node kullanımı, quorum sağlamak için gereklidir.

## Kube API Server

1. **Kube API Server Nedir?**
    - **Tanım:** Kube API Server, Kubernetes kontrol düzleminin (control plane) ana bileşenidir ve tüm diğer kontrol düzlemi bileşenleriyle iletişim kurarak cluster yönetimini sağlar.
    - **Görev:** Kubernetes API'sini sunar ve tüm isteklerin merkezi giriş noktasıdır. Cluster'ın durumunu yönetir ve istekleri etcd'ye yazar veya okur.
2. **Temel Görev ve Sorumluluklar:**
    - **API İsteklerini İşleme:** `kubectl`, diğer bileşenler ve harici istemcilerden gelen API isteklerini alır ve uygun şekilde yönlendirir.
    - **Validasyon ve Yetkilendirme:**
        - **Authentication (Kimlik Doğrulama):** İsteklerin kimliğini doğrular.
        - **Authorization (Yetkilendirme):** İsteklerin izinlerini kontrol eder (RBAC, ABAC, vb.).
        - **Admission Control:** İstekleri çeşitli politikalar ve kurallar üzerinden değerlendirir ve gerektiğinde değişiklikler yapar veya reddeder.
    - **Etcd ile İletişim:** Cluster durumunu kalıcı olarak saklamak için etcd veritabanına okuma ve yazma işlemleri yapar.
    - **Diğer Kontrol Düzlemi Bileşenleriyle İletişim:** Scheduler ve Controller Manager gibi bileşenlerle koordineli çalışır.
3. **Mimari ve Bileşenler:**
    - **RESTful API Arayüzü:** Tüm cluster bileşenleri ve istemcilerle iletişim kurmak için REST API sunar.
    - **Storage Backend:** Varsayılan olarak etcd kullanılır, tüm cluster durumu burada saklanır.
    - **Extensibility (Genişletilebilirlik):** Custom Resource Definitions (CRDs) ve API aggregation aracılığıyla API'yi genişletme imkanı sağlar.
    - **Admission Controllers:** İsteklerin cluster'a uygulanmadan önce modifiye edilmesini veya reddedilmesini sağlayan eklentiler. Örneğin:
        - **NamespaceLifecycle:** Namespace'lerin yaşam döngüsünü yönetir.
        - **ResourceQuota:** Kaynak kotalarını uygular.
        - **DefaultStorageClass:** Depolama için varsayılan sınıfı atar.
4. **Konfigürasyon ve Başlatma Parametreleri:**
    - **Komut Satırı Bayrakları:**
        - `-etcd-servers`: Etcd sunucularının adreslerini belirtir.
        - `-secure-port`: Güvenli API sunucusunun dinlediği portu ayarlar (varsayılan `6443`).
        - `-authorization-mode`: Yetkilendirme modlarını belirtir (örn. `RBAC`, `Node`, `Webhook`).
        - `-authentication-token-webhook-config-file`: Harici kimlik doğrulama için webhook yapılandırmasını belirtir.
        - `-audit-log-path`: Denetim (audit) loglarının yazılacağı dosyayı belirtir.
    - **Konfigürasyon Dosyaları:**
        - **api-server.yaml:** API sunucusunun pod tanımını içerir (genellikle kube-system namespace'inde bulunur).
        - **Admission Controller Yapılandırması:** Bazı denetim mekanizmaları için ek yapılandırma dosyaları gerekebilir.
5. **Güvenlik Özellikleri:**
    - **TLS Şifreleme:**
        - Tüm iletişimler TLS üzerinden şifrelenir.
        - Sertifikaların ve anahtarların doğru bir şekilde yönetilmesi kritik öneme sahiptir.
    - **Authentication (Kimlik Doğrulama):**
        - **Client Certificates:** Kullanıcıların sertifikaları ile doğrulanması.
        - **Bearer Tokens:** Token tabanlı kimlik doğrulama.
        - **OpenID Connect:** Harici kimlik sağlayıcıları ile entegrasyon.
    - **Authorization (Yetkilendirme):**
        - **RBAC (Role-Based Access Control):** Rol tabanlı erişim kontrolü sağlar.
        - **ABAC (Attribute-Based Access Control):** Öznitelik tabanlı erişim kontrolü.
        - **Webhook:** Harici servisler aracılığıyla yetkilendirme kararları alınabilir.
    - **Admission Control:**
        - Güvenlik politikalarını ve kısıtlamaları uygulamak için kullanılır.
        - Örneğin, pod'ların belirli node'larda çalışmasını engellemek veya belirli görüntülerin kullanılmasını zorunlu kılmak.
    - **Denetim Logları (Audit Logs):**
        - Cluster üzerinde gerçekleşen tüm işlemlerin kaydını tutar.
        - Güvenlik olaylarının izlenmesi ve denetlenmesi için kullanılır.
6. **Yüksek Erişilebilirlik (High Availability):**
    - **Çoklu API Server Kopyaları:**
        - API sunucusunun birden fazla instance'ı farklı node'larda çalıştırılarak yüksek erişilebilirlik sağlanır.
        - **Load Balancer:** İstekleri farklı API sunucusu instance'larına dağıtmak için kullanılır.
    - **Etcd ile Uyum:**
        - Tüm API sunucuları aynı etcd cluster'ına bağlanır ve verileri senkronize bir şekilde kullanır.
    - **Leader Election:**
        - Bazı işlemler için (örn. controller'lar) lider seçimi mekanizmaları kullanılır.
7. **Performans ve Ölçeklenebilirlik:**
    - **API İsteklerinin Optimizasyonu:**
        - **Caching:** Sık kullanılan verilerin cache mekanizmalarıyla tutulması.
        - **Watch Mechanism:** Kaynaklardaki değişiklikleri etkin bir şekilde izlemek için kullanılır.
    - **Resource Limiting:**
        - API sunucusunun CPU ve bellek kullanımı izlenmeli ve gerektiğinde sınırlandırılmalıdır.
    - **Horizontal Scaling:**
        - Artan yükü karşılamak için API sunucusu instance'larının sayısı artırılabilir.
    - **Profiling ve Monitoring:**
        - **Metrics Endpoint:** Prometheus gibi araçlarla entegrasyon için metrikler sunar.
        - **Logging:** Detaylı loglama ile performans sorunları ve hatalar tespit edilebilir.
8. **Sık Kullanılan Komutlar ve Etkileşimler:**
    - **API Sunucusunun Sağlık Durumu Kontrolü:**
        - `kubectl get componentstatuses`
        - `curl -k https://<api-server-ip>:6443/healthz`
    - **API Kaynaklarının Listelenmesi:**
        - `kubectl api-resources`
        - `kubectl api-versions`
    - **API Sunucusu Logları:**
        - `kubectl logs -n kube-system kube-apiserver-<node-name>`
    - **Sertifika Kontrolü:**
        - Sertifikaların süresinin ve geçerliliğinin düzenli olarak kontrol edilmesi önemlidir.
9. **Ortak Sorunlar ve Çözümler:**
    - **Bağlantı Hataları:**
        - **Sorun:** `kubectl` komutları zaman aşımına uğruyor veya API sunucusuna bağlanamıyor.
        - **Çözüm:** API sunucusunun çalıştığını ve doğru portlarda dinlediğini doğrulayın; network ve firewall ayarlarını kontrol edin.
    - **Kimlik Doğrulama/Yetkilendirme Sorunları:**
        - **Sorun:** Kullanıcılar veya servisler istenen kaynaklara erişemiyor.
        - **Çözüm:** RBAC politikalarını ve sertifikaları kontrol edin; gerekli izinlerin doğru bir şekilde tanımlandığından emin olun.
    - **Yüksek CPU/Bellek Kullanımı:**
        - **Sorun:** API sunucusu performans sorunları yaşıyor.
        - **Çözüm:** Yükü izleyin, gerekiyorsa daha fazla instance ekleyin veya mevcut kaynakları artırın; potansiyel olarak yoğun isteklerin kaynağını tespit edin ve optimize edin.
    - **Etcd Bağlantı Sorunları:**
        - **Sorun:** API sunucusu etcd ile iletişim kuramıyor.
        - **Çözüm:** Etcd sunucularının sağlıklı olduğunu ve doğru adreslerin kullanıldığını doğrulayın; network bağlantılarını ve sertifikaları kontrol edin.
10. **En İyi Uygulamalar:**
    - **Güncellemeleri ve Yedeklemeleri Planlayın:**
        - API sunucusu ve etcd için düzenli yedeklemeler ve planlı güncellemeler yapın.
    - **Güvenliği Ön Planda Tutun:**
        - Sertifikaları düzenli olarak yenileyin ve güçlü kimlik doğrulama/yetkilendirme mekanizmaları kullanın.
    - **Monitoring ve Alerting:**
        - API sunucusunun performansını ve sağlığını izlemek için monitoring araçları kullanın; anormallikler için uyarılar tanımlayın.
    - **Dokümantasyonu Takip Edin:**
        - Kubernetes'in resmi dokümantasyonunu ve en son sürüm notlarını takip ederek güncel kalın.
    - **Test Ortamları Kullanın:**
        - Değişiklikleri ve güncellemeleri öncelikle test ortamlarında deneyin.

## Kube Controller Manager

1. **Kube Controller Manager Nedir?**
    - **Tanım:** Kube Controller Manager, Kubernetes kontrol düzleminde çalışan ve cluster durumunu istenen duruma getiren çeşitli kontrol döngülerini (controllers) yöneten bir bileşendir.
    - **Görev:** Cluster’daki kaynakların durumu (örneğin, Pod, Node, Service) ile istenen durum arasındaki farkları sürekli olarak kontrol eder ve bu farkları düzeltir.
2. **Temel Görev ve Sorumluluklar:**
    - **Kontrol Döngüleri:** Kube Controller Manager, birden fazla kontrol döngüsünü (controller) tek bir süreçte çalıştırır. Her kontrol döngüsü, belirli bir Kubernetes kaynağını yönetir.
    - **Uzlaştırma:** Her kontrol döngüsü, kaynakların mevcut durumunu istenen duruma uyacak şekilde sürekli olarak uzlaştırır. Örneğin, bir Pod başarısız olduğunda, ReplicaSet Controller bu Pod’u yeniden oluşturur.
3. **Önemli Kontrol Döngüleri:**
    - **Node Controller:** Node’ların durumunu izler. Node çalışmaz hale gelirse, üzerinde çalışan Pod’ların başka bir node’a taşınmasını sağlar.
    - **Replication Controller:** Belirli sayıda Pod’un her zaman çalışır durumda olmasını sağlar. Eğer Pod’lar ölürse, yeni Pod’lar oluşturur.
    - **Endpoints Controller:** Service ile ilişkilendirilmiş Pod’ları takip eder ve Endpoints objesini günceller.
    - **Service Account & Token Controller:** Her yeni namespace için varsayılan service account oluşturur ve bu account’a token atar.
    - **Job Controller:** İşlemlerin başarılı bir şekilde tamamlanmasını sağlar. Tamamlanmamış veya başarısız işler için yeniden deneme yapılmasını yönetir.
    - **DaemonSet Controller:** Belirli bir node grubunda her node’da bir Pod çalıştırılmasını sağlar.
    - **Garbage Collector:** Sahipliği olmayan (orphaned) kaynakları temizler. Örneğin, silinmiş bir ReplicaSet’e ait Pod’ları kaldırır.
4. **Kube Controller Manager Özellikleri:**
    - **Single Process:** Tüm kontrol döngüleri tek bir Kube Controller Manager sürecinde çalışır.
    - **Leader Election:** Çoklu controller manager kopyaları çalıştırılabilir, ancak sadece biri etkin (leader) olarak çalışır. Liderlik geçişi otomatik olarak yapılır.
    - **Custom Controllers:** Kullanıcılar kendi özel kontrol döngülerini yazabilir ve cluster’a entegre edebilirler.
5. **Konfigürasyon ve Yönetim:**
    - **Komut Satırı Bayrakları:**
        - `-controllers`: Hangi kontrol döngülerinin çalıştırılacağını belirler. Örneğin, `-controllers=*,-daemonset` komutu, tüm kontrol döngülerini çalıştırır, ancak DaemonSet controller'ı hariç tutar.
        - `-leader-elect`: Lider seçimi mekanizmasını etkinleştirir.
        - `-cluster-cidr`: Cluster içi pod ağını belirtir, network policy’ler ve IP adres atamaları için kullanılır.
        - `-service-cluster-ip-range`: Servisler için kullanılacak IP aralığını belirler.
    - **Yüksek Erişilebilirlik (HA):**
        - Birden fazla kube-controller-manager instance’ı çalıştırarak yüksek erişilebilirlik sağlanır.
        - **Leader Election:** Birden fazla kopya çalıştırıldığında, sadece bir kopya leader olarak çalışır ve diğerleri standby olarak bekler. Leader başarısız olursa, bir başkası lider olur.
6. **Performans ve Ölçeklenebilirlik:**
    - **Optimizasyon:** Kube Controller Manager’ın performansı, çalıştırdığı kontrol döngülerinin sayısı ve bu döngülerin gerektirdiği kaynaklara bağlıdır. Gereksiz kontrol döngüleri devre dışı bırakılabilir.
    - **Monitoring:** Prometheus gibi araçlarla metrikler izlenebilir, bu sayede kontrol döngülerinin performansı takip edilebilir.
7. **Sık Kullanılan Komutlar ve Etkileşimler:**
    - **Kontrol Döngülerini Görüntüleme:**
        - `kubectl describe controllerrevisions` komutu ile belirli bir kaynağın hangi kontrol döngüleri tarafından yönetildiğini görebilirsiniz.
    - **Log İnceleme:**
        - `kubectl logs -n kube-system kube-controller-manager-<node-name>` komutu ile kube-controller-manager loglarını inceleyebilirsiniz.
    - **Lider Durumunu Kontrol Etme:**
        - `kubectl get endpoints kube-controller-manager -n kube-system -o yaml` komutu ile mevcut lideri kontrol edebilirsiniz.
8. **Ortak Sorunlar ve Çözümler:**
    - **Uzlaştırma Gecikmeleri:**
        - **Sorun:** Pod veya Node gibi kaynakların istenen duruma getirilmesi zaman alabilir.
        - **Çözüm:** Kube Controller Manager’ın performansını izleyin, yoğun istekleri tespit edin ve gerekirse optimize edin.
    - **Lider Seçimi Sorunları:**
        - **Sorun:** Çoklu controller manager instance’ları arasında lider seçimi sorunları yaşanabilir.
        - **Çözüm:** Leader election mekanizmasını ve ilgili ayarları kontrol edin, etcd bağlantılarını ve quorum’u doğrulayın.
    - **Kapasite ve Kaynak Sorunları:**
        - **Sorun:** Kube Controller Manager’ın çalışması için yeterli CPU veya bellek kaynakları bulunmayabilir.
        - **Çözüm:** Kaynak limitlerini gözden geçirin ve gerekirse daha fazla kaynak ayırın.
9. **En İyi Uygulamalar:**
    - **Leader Election Kullanımı:** Yüksek erişilebilirlik sağlamak için leader election’ı etkinleştirin.
    - **Monitoring:** Kube Controller Manager’ın performansını ve lider seçim süreçlerini izlemek için Prometheus gibi araçlar kullanın.
    - **Kontrol Döngülerini Yönetme:** Sadece gerekli kontrol döngülerini çalıştırın; gereksiz döngüleri devre dışı bırakmak, performansı artırabilir.
    - **Güncellemeleri Planlama:** Kube Controller Manager ve kontrol döngülerinin düzenli olarak güncellenmesini planlayın.

## Node Controller

1. **Node Controller Nedir?**
    - **Tanım:** Node Controller, Kubernetes kontrol düzleminde çalışan ve cluster’daki node’ların durumunu izleyen, gerektiğinde müdahale eden bir kontrol döngüsüdür (controller).
    - **Görev:** Node’ların sağlık durumunu izler, node’lar arasındaki dengeyi sağlar, başarısız olan node’lara tepki verir ve bu node’lar üzerindeki Pod’ların yeniden programlanmasını sağlar.
2. **Temel Görev ve Sorumluluklar:**
    - **Node Sağlık Kontrolü:**
        - Node Controller, her node’un düzenli aralıklarla sağlıklı olup olmadığını kontrol eder.
        - **Node Status:** Node'ların CPU, bellek, disk durumu ve diğer sağlık metriklerini takip eder.
    - **Heartbeats (Kalp Atışları):**
        - Kubelet, API Server'a düzenli olarak `NodeStatus` bilgisi gönderir. Bu bilgilere "heartbeat" denir.
        - Node Controller, bu heartbeat'leri izler ve belirli bir süre (varsayılan 5 dakika) boyunca heartbeat almazsa node'u "NotReady" durumuna getirir.
    - **Node Kapanması ve Yeniden Başlatılması:**
        - Node, belirli bir süre boyunca yanıt vermezse (örn. donanımsal sorunlar veya network problemleri nedeniyle), Node Controller bu node'u kullanılamaz olarak işaretler.
        - **Pod Yeniden Programlama:** Node Controller, kullanılamaz hale gelen bir node üzerindeki Pod'ları diğer uygun node'lara yeniden programlar.
3. **Node Durumları:**
    - **Ready:** Node, Pod'ları çalıştırmaya hazır ve sağlıklı durumda.
    - **NotReady:** Node, çalışmıyor veya node'dan sağlık sinyali alınmıyor.
    - **Unknown:** Node hakkında bilgi alınamıyor; genellikle network sorunları nedeniyle bu durum oluşur.
    - **OutOfDisk:** Node'un disk alanı dolmuş, yeni Pod'ları çalıştırmak için yeterli disk alanı yok.
    - **MemoryPressure:** Node'da bellek sıkıntısı var, mevcut Pod'lar bellek tüketimini aşmış olabilir.
    - **DiskPressure:** Node'da disk I/O ile ilgili sorunlar var, bu durum node'un performansını etkileyebilir.
4. **Node Controller’ın Tepkileri:**
    - **Pod'ların Yeniden Programlanması:**
        - **Grace Period (Geri Alma Süresi):** Node, belirli bir süre boyunca (varsayılan 5 dakika) sağlıklı heartbeat göndermezse, üzerindeki Pod'lar "evicted" olarak işaretlenir ve diğer node'lara taşınır.
        - **Pod Eviction:** Kube-scheduler, bu Pod'ları uygun olan diğer node'lara yeniden yerleştirir.
    - **Node Marking (Node İşaretleme):**
        - Node, "NotReady" veya "Unknown" olarak işaretlendiğinde, bu node üzerindeki yeni Pod yerleşimlerine izin verilmez.
    - **Cloud Provider Entegrasyonu:**
        - Node Controller, belirli cloud provider’larla entegre çalışarak (örn. AWS, GCP) kullanılmayan veya başarısız olan node’ların otomatik olarak kaldırılmasını sağlayabilir.
        - Cloud provider API’leri üzerinden node’un fiziksel olarak silinmesini tetikleyebilir.
5. **Konfigürasyon ve Yönetim:**
    - **Komut Satırı Bayrakları:**
        - `-node-monitor-grace-period`: Node’un başarısız olarak değerlendirilmesi için geçen süre (varsayılan: 40 saniye).
        - `-pod-eviction-timeout`: Bir node başarısız olduğunda, bu node üzerindeki Pod’ların tahliye edilmesi için geçen süre (varsayılan: 5 dakika).
        - `-node-eviction-rate`: Node başarısızlıklarında Pod’ların ne hızla tahliye edileceğini belirler.
    - **Yüksek Erişilebilirlik (HA):**
        - Node Controller, çok sayıda node’u yönetmek için optimize edilmiştir. Büyük bir cluster’da, node’ların durumu sürekli olarak izlenir ve hızlı bir şekilde müdahale edilir.
        - **Leader Election:** Yüksek erişilebilirlik için Node Controller, leader election mekanizması ile çalışır. Birden fazla instance çalıştırılabilir, ancak yalnızca biri lider olarak görev yapar.
6. **Monitoring ve İzleme:**
    - **Node Durumu İzleme:**
        - `kubectl get nodes` komutu ile tüm node’ların mevcut durumunu görebilirsiniz.
    - **Node Logları:**
        - Her bir node’un logları `kubectl logs` komutu ile incelenebilir, bu loglar node’un sağlık durumu hakkında bilgi verir.
    - **Alerting:**
        - Prometheus gibi izleme araçlarıyla node durumunu izleyebilir ve anormal durumlar için uyarılar oluşturabilirsiniz.
    - **Metrics:**
        - Node'ların durumu ve performansı hakkında detaylı bilgi almak için metrics-server gibi araçları kullanabilirsiniz.
7. **Ortak Sorunlar ve Çözümler:**
    - **Node NotReady veya Unknown Durumunda:**
        - **Sorun:** Node belirli bir süre boyunca sağlıklı heartbeat göndermez.
        - **Çözüm:** Node’un fiziksel durumu, network bağlantıları ve kubelet logları incelenmeli; node’un yeniden başlatılması veya sistemin düzeltilmesi gerekebilir.
    - **Pod Yeniden Programlama Gecikmeleri:**
        - **Sorun:** Pod’lar, node başarısız olduktan sonra hızlıca yeniden programlanmıyor.
        - **Çözüm:** `-pod-eviction-timeout` gibi ayarlar gözden geçirilmeli ve gereksinimlere uygun olarak optimize edilmelidir.
    - **Disk/Memory Pressure Uyarıları:**
        - **Sorun:** Node üzerindeki Pod’lar disk veya bellek limitlerini aşıyor.
        - **Çözüm:** Pod kaynak talepleri ve limitleri gözden geçirilmeli, gerekirse node kaynakları artırılmalı veya Pod dağılımı optimize edilmelidir.
8. **En İyi Uygulamalar:**
    - **Regular Monitoring:** Node’ların sağlık durumu ve performansı düzenli olarak izlenmeli; sorunlar erken tespit edilmelidir.
    - **HA Kurulumlar:** Node Controller’ın yüksek erişilebilirliğini sağlamak için birden fazla instance çalıştırın.
    - **Proactive Resource Management:** Node’ların kaynak kullanımları yakından izlenmeli, kritik durumlar oluşmadan önce müdahale edilmelidir.
    - **Cloud Provider Integration:** Cloud ortamında çalışıyorsanız, Node Controller’ın cloud provider API’leri ile entegre çalıştığından emin olun.

## Replication Controller

1. **Replication Controller Nedir?**
    - **Tanım:** Replication Controller (RC), Kubernetes'te belirli bir sayıda Pod'un her zaman çalışır durumda olmasını garanti eden bir kontrol döngüsüdür. İstenilen sayıda Pod’un çalışmasını sağlar ve Pod’ların durumunu sürekli olarak izler.
    - **Görev:** Herhangi bir nedenden dolayı bir veya daha fazla Pod devre dışı kalırsa, Replication Controller eksik olan Pod'ları yeniden başlatır. Aynı zamanda, fazladan çalışan Pod’lar varsa bunları da sonlandırır.
2. **Replication Controller’ın Temel Görevleri:**
    - **Desired State Management (İstenen Durum Yönetimi):**
        - Replication Controller, Pod’ların sayısını "desired state" (istenen durum) ile eşleşecek şekilde ayarlar.
    - **Pod Yaratma ve Silme:**
        - Eğer mevcut Pod sayısı, istenen Pod sayısından azsa, yeni Pod’lar oluşturur.
        - Eğer mevcut Pod sayısı, istenen Pod sayısından fazlaysa, fazla Pod’ları siler.
    - **Pod Sağlık Kontrolü:**
        - Pod’ların sağlığını izler ve başarısız olan Pod’ları yenileriyle değiştirir.
    - **Kapsama Alanı:**
        - Replication Controller sadece belirli bir node üzerinde değil, tüm cluster’daki Pod’ları yönetir.
3. **Replication Controller ve Replication Set Arasındaki Fark:**
    - **Replication Controller:**
        - Eski bir mekanizmadır; `replicas` sayısını belirtir ve belirli etiketlere sahip Pod’ları izler.
        - Sadece tam eşleşen etiketleri destekler.
    - **Replication Set:**
        - Replication Controller’ın yerini almıştır; daha geniş etiket seçici (label selector) desteği sunar.
        - `matchLabels` ve `matchExpressions` gibi gelişmiş etiket eşleme mekanizmalarını destekler.
4. **Replication Controller’ın Yapı Taşları:**
    - **Pod Template:**
        - Yeni Pod’lar oluşturulurken kullanılacak yapılandırmayı içerir. Bu yapı, Pod’un container, volume, network ayarları gibi tüm detaylarını içerir.
    - **Label Selector:**
        - Replication Controller, hangi Pod’ları izleyeceğini belirlemek için label selector kullanır. Bu etiketler, Pod’ların Replication Controller tarafından izlenmesini sağlar.
    - **Replicas:**
        - Replication Controller’ın kaç tane Pod’un çalışır durumda olmasını istediğinizi belirtir. Örneğin, `replicas: 3` demek, her zaman 3 adet Pod’un çalışması gerektiği anlamına gelir.
5. **Replication Controller Yapılandırması:**
    - **Temel Yapı:**
        - `apiVersion`: Replication Controller’ın hangi API versiyonu kullanılarak oluşturulacağını belirtir. Genellikle `v1` kullanılır.
        - `kind`: Bu değeri `ReplicationController` olarak ayarlamak gerekir.
        - `metadata`: Replication Controller için isim ve etiketler gibi meta bilgileri içerir.
        - `spec`: Replication Controller’ın detaylı ayarlarını içerir, özellikle `replicas` sayısı ve `template` yapısı burada tanımlanır.
    - **Örnek YAML:**

    ```yaml
    apiVersion: v1
    kind: ReplicationController
    metadata:
    name: my-replication-controller
    spec:
    replicas: 3
    selector:
    app: my-app
    template:
    metadata:
    labels:
    app: my-app
    spec:
    containers:
    - name: my-container
    image: my-image:latest
    ports:
    - containerPort: 80
    ```

   - **Özellikler:**
        - `replicas`: Her zaman kaç tane Pod’un çalışır durumda olması gerektiğini belirtir.
        - `selector`: Replication Controller’ın izleyeceği Pod’ları tanımlamak için kullanılan etiketler.
        - `template`: Yeni Pod’lar oluşturulurken kullanılacak olan yapı.
  
6. **Replication Controller’ın Kullanımı:**
    - **Oluşturma:**
        - `kubectl create -f replication-controller.yaml` komutuyla bir Replication Controller oluşturabilirsiniz.
    - **Durum Kontrolü:**
        - `kubectl get replicationcontroller` komutu ile Replication Controller’ın durumu görüntülenebilir.
        - `kubectl describe replicationcontroller <name>` komutuyla daha detaylı bilgi alabilirsiniz.
    - **Pod Yönetimi:**
        - Eğer bir Pod silinirse, Replication Controller hemen yeni bir Pod oluşturarak durumu dengelemeye çalışır.
        - Eğer bir Pod manuel olarak oluşturulmuşsa ve Replication Controller tarafından yönetilmiyorsa, bu Pod üzerinde Replication Controller’ın kontrolü yoktur.
7. **Scaling (Ölçeklendirme):**
    - **Manuel Ölçeklendirme:**
        - `kubectl scale --replicas=5 replicationcontroller <name>` komutuyla Pod sayısını manuel olarak değiştirebilirsiniz.
    - **Otomatik Ölçeklendirme:**
        - Replication Controller, doğrudan otomatik ölçeklendirme yeteneğine sahip değildir, ancak Horizontal Pod Autoscaler (HPA) gibi araçlarla entegrasyon sağlanabilir.
8. **Ortak Sorunlar ve Çözümler:**
    - **Pod'ların Yetersiz Çalıştırılması:**
        - **Sorun:** Replication Controller tarafından tanımlanan sayıda Pod çalışmıyor.
        - **Çözüm:** Pod şablonunu, cluster kaynaklarını ve node durumunu kontrol edin; Pod'ların başlatılmasına engel olan hataları inceleyin.
    - **Pod Çakışmaları:**
        - **Sorun:** Aynı etiketlere sahip başka Pod’lar manuel olarak oluşturulmuş olabilir ve Replication Controller bunları yönetmeye çalışabilir.
        - **Çözüm:** Replication Controller’ın kullandığı etiketler için doğru label selector'ların tanımlandığından emin olun.
9. **En İyi Uygulamalar:**
    - **Label Yönetimi:** Etiketlerinizi dikkatlice yönetin; Replication Controller’ın doğru Pod’ları izlediğinden emin olun.
    - **Şablon Optimizasyonu:** Pod template’ini, ihtiyaçlarınıza göre optimize edin; gereksiz kaynak kullanımı veya çakışmaların önüne geçin.
    - **Durum İzleme:** Replication Controller’ın durumunu ve ilgili Pod’ları düzenli olarak izleyin; özellikle başarısızlık durumlarında hızlıca müdahale edin.
    - **Replication Set Kullanımı:** Yeni projelerde veya daha karmaşık etiketleme gereksinimleri olan durumlarda, Replication Set’i tercih edin.

## ReplicaSet

1. **ReplicaSet Nedir?**
    - **Tanım:** ReplicaSet, Kubernetes'te belirli sayıda Pod'un her zaman çalışır durumda olmasını sağlamak için kullanılan bir kontrol döngüsüdür. ReplicaSet, Pod'ların sayısını takip eder ve belirli bir sayıdaki Pod'un her zaman aktif olmasını garanti eder.
    - **Görev:** Pod'ların ölmesi veya kaybolması durumunda, ReplicaSet eksik olan Pod'ları yeniden oluşturur. Aynı şekilde, fazla Pod'ları da kapatır, böylece her zaman tanımlanan sayıda Pod çalışır durumda kalır.
2. **ReplicaSet ile Deployment Arasındaki Fark:**
    - **ReplicaSet:** Daha eski bir kontrol mekanizmasıdır. Pod’ları yönetir ancak güncellemeler ve roll-back işlemleri için sınırlı yeteneklere sahiptir.
    - **Deployment:** Deployment, ReplicaSet’i kapsayan ve güncellemeleri, roll-back işlemlerini daha etkin bir şekilde yöneten daha yüksek seviyeli bir yapılandırmadır. Deployment, ReplicaSet kullanarak Pod’ları yönetir.
3. **ReplicaSet Tanımı (YAML):**

    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
    name: example-replicaset
    labels:
        app: example
    spec:
    replicas: 3
    selector:
        matchLabels:
        app: example
    template:
        metadata:
        labels:
            app: example
        spec:
        containers:
        - name: example-container
            image: nginx:latest
            ports:
            - containerPort: 80
    ```

#### ReplicaSet Tanımı İçindeki Alanlar ve Olası Değerler

| **Alan**     | **Açıklama**                                                                    | **Olası Değerler / Seçenekler**                                                                                                                |
| ------------ | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiVersion` | Kubernetes API sürümünü belirtir.                                               | - `apps/v1`                                                                                                                                    |
| `kind`       | Kaynak türünü belirtir.                                                         | - `ReplicaSet`                                                                                                                                 |
| `metadata`   | ReplicaSet’e ait meta bilgileri içerir, örneğin ad ve etiketler.                | - `name`: ReplicaSet adı (örn. `example-replicaset`)  <br>- `labels`: ReplicaSet için etiketler (örn. `app: example`)                          |
| `spec`       | ReplicaSet’in nasıl çalışacağını tanımlar.                                      | - `replicas`: Çalıştırılacak Pod sayısı (örn. `3`)  <br>- `selector`: Pod’ları seçmek için kullanılan etiketler  <br>- `template`: Pod şablonu |
| `replicas`   | Çalışır durumda olması istenen Pod sayısını belirtir.                           | - Sayısal değer (örn. `3`, `5`, `10`)                                                                                                          |
| `selector`   | ReplicaSet’in hangi Pod’ları yöneteceğini belirlemek için kullanılan etiketler. | - `matchLabels`: Belirli etiketlere sahip Pod’ları seçmek için kullanılır (örn. `app: example`)                                                |
| `template`   | Yeni Pod'ların nasıl oluşturulacağını belirler (Pod şablonu).                   | - `metadata`: Pod için etiketler ve ad tanımları  <br>- `spec`: Pod’un container, volume, restart policy vb. özelliklerini içerir              |
| `containers` | ReplicaSet’in yöneteceği Pod'lar içinde çalışacak container’ları tanımlar.      | - `name`: Container adı (örn. `example-container`)  <br>- `image`: Container image’ı (örn. `nginx:latest`)  <br>- Diğer container alanları     |

#### Örnek ReplicaSet Yapılandırması ve Alan Seçenekleri

| **Alan**                   | **Açıklama**                                              | **Örnek Değerler**                                                                         |
| -------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `apiVersion`               | Kaynağın hangi API sürümüyle tanımlandığını belirtir.     | - `apps/v1`                                                                                |
| `kind`                     | Kaynağın türü (ReplicaSet).                               | - `ReplicaSet`                                                                             |
| `metadata.name`            | ReplicaSet’in adını belirtir.                             | - `example-replicaset`                                                                     |
| `replicas`                 | Çalışması gereken Pod sayısını tanımlar.                  | - `3`, `5`, `10`                                                                           |
| `selector.matchLabels`     | ReplicaSet’in yöneteceği Pod’ları seçmek için kullanılır. | - `app: example`                                                                           |
| `template.metadata.labels` | Pod şablonundaki Pod’lar için etiketler belirler.         | - `app: example`                                                                           |
| `template.spec.containers` | Pod şablonundaki container’ları tanımlar.                 | - `name: example-container`  <br>- `image: nginx:latest`  <br>- `ports: containerPort: 80` |
| `template.spec`            | Pod’ların çalışma özelliklerini belirtir.                 | - `containers`, `volumes`, `restartPolicy`, vb.                                            |

4. **ReplicaSet’in Çalışma Prensibi:**
    - **Pod İzleme ve Yönetim:**
        - ReplicaSet, tanımlanan sayıda Pod’un çalıştığından emin olmak için Pod’ları sürekli olarak izler.
    - **Pod Yeniden Oluşturma:**
        - Eğer bir Pod başarısız olursa veya manuel olarak silinirse, ReplicaSet bu eksikliği giderir ve yeni bir Pod oluşturur.
    - **Fazla Pod'ları Kapatma:**
        - Eğer Pod sayısı, belirtilen `replicas` değerinden fazla ise, ReplicaSet fazla Pod’ları kapatır.
5. **ReplicaSet Yönetimi:**
    - **ReplicaSet Oluşturma:**
        - `kubectl apply -f replicaset.yaml` komutuyla YAML dosyasındaki ReplicaSet tanımını uygulayabilirsiniz.
    - **ReplicaSet İzleme:**
        - `kubectl get replicasets` komutuyla mevcut ReplicaSet'lerin durumu görüntülenir.
    - **Pod Sayısını Güncelleme:**
        - `kubectl scale --replicas=5 replicaset example-replicaset` komutuyla ReplicaSet’teki Pod sayısını güncelleyebilirsiniz.
    - **ReplicaSet Silme:**
        - `kubectl delete replicaset example-replicaset` komutuyla ReplicaSet’i ve onun yönettiği Pod’ları silebilirsiniz.
6. **ReplicaSet’in Avantajları:**
    - **Otomatik Ölçekleme:**
        - ReplicaSet, Pod sayısını otomatik olarak yönetir ve sistemde gerekli sayıda Pod’un çalışır durumda olmasını sağlar.
    - **Yüksek Erişilebilirlik:**
        - Uygulamanın kesintisiz çalışmasını sağlar. Pod’lar başarısız olduğunda veya kapatıldığında, ReplicaSet bu eksiklikleri hızla giderir.
    - **Yönetim Kolaylığı:**
        - Birden fazla Pod’u tek bir yapı üzerinden yönetmenizi sağlar. Bu, uygulamanın dağıtımını ve yönetimini kolaylaştırır.

## Kube-Scheduler

1. **Kube-Scheduler’ın Görevi:**
    - **Pod Ataması:** Kube-scheduler, yeni oluşturulan Pod’ları uygun Worker Node’lara atar. Bu atama işlemi, cluster’daki mevcut kaynaklara (CPU, bellek) ve diğer kısıtlamalara göre yapılır.
2. **Scheduler Süreci:**
    - **Pending Pod:** Bir Pod, oluşturulduğunda önce "Pending" durumuna geçer. Scheduler, bu Pod’un hangi node’da çalışacağına karar verir.
    - **Filtering:** Scheduler, Pod’u çalıştırabilecek uygun node’ları belirler. Kısıtlamalar (örn. node’un kaynak durumu, affinity/anti-affinity kuralları) bu aşamada göz önünde bulundurulur.
    - **Scoring:** Uygun node’lar arasında bir seçim yapılır. En iyi skor alan node, Pod’u çalıştırmak için seçilir. Skorlama, node’un mevcut yük durumu, pod yerleşimi gibi faktörlere göre yapılır.
3. **Scheduler Stratejileri:**
    - **Binpacking:** Pod’lar, node’ların kaynaklarını en verimli şekilde kullanmak için sıkıca yerleştirilir.
    - **Spread:** Pod’lar, yüksek erişilebilirlik için farklı node’lara dağıtılır.
4. **Custom Scheduler:**
    - **Custom Policies:** Kullanıcılar, özel scheduler stratejileri oluşturabilir. Örneğin, belirli Pod’ların belirli node’larda çalışmasını zorunlu kılmak.
    - **Multiple Schedulers:** Cluster’da birden fazla scheduler çalıştırılabilir. Özel bir scheduler, Pod tanımında belirtilerek seçilebilir.
5. **Affinity ve Anti-Affinity:**
    - **Node Affinity:** Pod’un belirli node’larda çalışmasını sağlar. `requiredDuringSchedulingIgnoredDuringExecution` gibi kurallar kullanılır.
    - **Pod Affinity/Anti-Affinity:** Pod’ların birbirine yakın veya uzak yerleştirilmesini sağlar. Aynı veya farklı node’larda çalışmasını zorunlu kılar.
6. **Preemption:**
    - **Preemption Mechanism:** Scheduler, önceden yerleştirilmiş düşük öncelikli Pod’ları kaldırarak daha yüksek öncelikli bir Pod’a yer açabilir.
7. **Scheduler Configuration:**
    - **Config File:** Scheduler’ın davranışını belirleyen ayarlar, config dosyasında yapılabilir. Burada policy, priorities, ve custom predicates gibi seçenekler tanımlanır.

## Kubelet

1. **Kubelet Nedir?**
    - **Tanım:** Kubelet, Kubernetes cluster'ındaki her node üzerinde çalışan bir ajan (agent) uygulamasıdır. Kubelet, node üzerindeki container’ların çalışmasını sağlar ve node’un durumunu kontrol eder.
    - **Görev:** Kubelet, Kubernetes API Server ile iletişim kurarak, node üzerinde çalıştırılması gereken Pod’ları alır ve bu Pod’ların çalışmasını sağlar. Aynı zamanda, node’un ve üzerindeki Pod’ların sağlık durumunu sürekli olarak izler.
2. **Kubelet’in Temel Görevleri:**
    - **Pod Yönetimi:**
        - Kubelet, Kubernetes API Server’dan aldığı Pod tanımlarını (manifest) alır ve bu Pod’ların node üzerinde çalışmasını sağlar.
        - **Container Runtime ile Entegrasyon:** Kubelet, Docker, containerd gibi container runtime’ları ile çalışarak container’ların başlatılmasını ve yönetilmesini sağlar.
    - **Pod Sağlık Kontrolü:**
        - Kubelet, Pod’ların ve container’ların sağlık durumunu izler. Eğer bir container başarısız olursa, Kubelet bu container’ı yeniden başlatır.
    - **Node Sağlık Kontrolü:**
        - Kubelet, node’un genel sağlık durumunu izler ve Kubernetes API Server’a düzenli olarak raporlar (node status, resource usage).
    - **Volume ve Secrets Yönetimi:**
        - Kubelet, Pod’ların ihtiyaç duyduğu volümleri bağlar ve Kubernetes Secret’larını Pod’lara ulaştırır.
3. **Kubelet’in Çalışma Prensibi:**
    - **Node Registration (Node Kaydı):**
        - Kubelet, node’u Kubernetes cluster’ına kaydeder ve API Server’a node’un özelliklerini bildirir (örneğin, CPU, bellek, disk kapasitesi).
    - **Pod Lifecycle Management (Pod Yaşam Döngüsü Yönetimi):**
        - Kubelet, API Server’dan Pod tanımlarını alır ve container runtime ile birlikte bu Pod’ları başlatır.
        - Pod’ların yaşam döngüsünü (başlatma, durdurma, yeniden başlatma) yönetir.
    - **Container Monitoring (Container İzleme):**
        - Kubelet, container’ların çalışıp çalışmadığını izler. Eğer bir container çökerse, Kubelet bu durumu tespit eder ve container’ı yeniden başlatır.
4. **Kubelet’in Yapı Taşları:**
    - **PodSpec:**
        - Kubelet, Pod tanımlarını PodSpec formatında alır. PodSpec, Pod’un nasıl çalıştırılacağını ve hangi container’ların oluşturulacağını belirler.
    - **Container Runtime Interface (CRI):**
        - Kubelet, CRI aracılığıyla container runtime (örneğin, Docker, containerd) ile iletişim kurar ve container’ları başlatır veya durdurur.
    - **Node Status Reporting:**
        - Kubelet, node’un durumunu, kullanılabilir kaynaklarını ve Pod’ların sağlık durumunu düzenli olarak API Server’a raporlar.
    - **cAdvisor Entegrasyonu:**
        - Kubelet, cAdvisor (Container Advisor) aracılığıyla node üzerindeki container’ların kaynak kullanımını izler ve raporlar (CPU, bellek, disk kullanımı).
5. **Kubelet Yapılandırması:**
    - **Komut Satırı Bayrakları:**
        - `-config`: Kubelet yapılandırma dosyasının yolunu belirtir.
        - `-pod-manifest-path`: Statik Pod tanımlarının bulunduğu dizini belirtir. Kubelet, bu dizindeki Pod tanımlarını otomatik olarak yükler.
        - `-kubeconfig`: API Server ile iletişim kurmak için kullanılacak kubeconfig dosyasının yolunu belirtir.
        - `-register-node`: Kubelet'in node’u Kubernetes cluster’ına kaydedip kaydetmeyeceğini belirtir (varsayılan: true).
        - `-container-runtime`: Kullanılacak container runtime türünü belirtir (örneğin, `docker`, `containerd`).
        - `-fail-swap-on`: Swap’ın etkin olup olmamasına göre Kubelet’in başlatılıp başlatılmayacağını belirler (varsayılan: true).
    - **Yapılandırma Dosyası:**
        - Kubelet'in yapılandırma dosyasında node’a özgü ayarlar (örneğin, maxPods, evictionThresholds) tanımlanır.
        - Örnek Yapılandırma:

        ```yaml
        apiVersion: kubelet.config.k8s.io/v1beta1
        kind: KubeletConfiguration
        address: 0.0.0.0
        staticPodPath: /etc/kubernetes/manifests
        clusterDNS:

        - 10.96.0.10
        clusterDomain: cluster.local
        ```

6. **Monitoring ve Loglama:**
    - **Kubelet Logları:**
        - Kubelet logları, node üzerinde neler olup bittiğini izlemek için kullanılır. Loglar, genellikle `/var/log/kubelet.log` dosyasında bulunur.
        - `journalctl -u kubelet` komutu ile Kubelet loglarına erişebilirsiniz.
    - **cAdvisor Metrikleri:**
        - Kubelet, cAdvisor üzerinden node’un kaynak kullanımını izler ve bu verileri Prometheus gibi izleme sistemlerine gönderebilir.
    - **Node Durumu İzleme:**
        - `kubectl get nodes` komutu ile node’ların genel durumu izlenebilir.
        - `kubectl describe node <node-name>` komutu ile belirli bir node’un detaylı durumu görülebilir.
7. **Sık Karşılaşılan Sorunlar ve Çözümler:**
    - **Kubelet’in API Server ile Bağlantı Sorunları:**
        - **Sorun:** Kubelet, API Server’a bağlanamıyor ve node "NotReady" durumunda görünüyor.
        - **Çözüm:** Kubeconfig dosyasını, API Server adresini ve ilgili sertifikaları kontrol edin. Network bağlantılarını doğrulayın.
    - **Pod’ların Başlatılamaması:**
        - **Sorun:** Kubelet, Pod’ları başlatamıyor veya container’lar sürekli olarak çöküyor.
        - **Çözüm:** Pod tanımını, container runtime loglarını ve kaynak kullanımını kontrol edin. Gerektiğinde Pod şablonlarını ve kaynak taleplerini düzenleyin.
    - **Node Disk veya Bellek Sorunları:**
        - **Sorun:** Kubelet, disk veya bellek tükenmesi nedeniyle Pod’ları çalıştırmayı durduruyor.
        - **Çözüm:** Node üzerindeki disk ve bellek kullanımını izleyin; gereksiz dosyaları temizleyin ve node kaynaklarını gerektiğinde artırın.
8. **En İyi Uygulamalar:**
    - **Kaynak Yönetimi:** Node’ların kaynak kullanımını dikkatle izleyin ve gerektiğinde kaynak limitleri ve taleplerini ayarlayın.
    - **Düzenli Güncellemeler:** Kubelet’i ve container runtime’ını düzenli olarak güncelleyin, güvenlik açıklarını kapatın.
    - **Swap Yönetimi:** Swap’ın devre dışı olduğundan emin olun, çünkü Kubernetes node’ları swap’ı etkin olarak çalıştırmayı önermez.
    - **Güvenlik:** Kubelet ve container runtime arasındaki iletişim şifrelenmelidir. Ayrıca, Kubelet’in yalnızca yetkili kullanıcılar tarafından erişilebilmesini sağlayın.

## Kube-proxy

1. **Kube-proxy Nedir?**
    - **Tanım:** Kube-proxy, Kubernetes cluster'ındaki her node üzerinde çalışan bir ağ proxy’sidir. Cluster içindeki network iletişimini sağlar ve Pod'lar arasındaki trafiği yönlendirir.
    - **Görev:** Kube-proxy, Kubernetes Service objelerini kullanarak, dış dünyadan gelen isteklerin doğru Pod’lara yönlendirilmesini sağlar. Ayrıca, cluster içindeki Pod’lar arasında da iletişimi düzenler.
2. **Kube-proxy’nin Temel Görevleri:**
    - **Service Discovery (Servis Keşfi):**
        - Kube-proxy, Kubernetes API Server’dan aldığı Service ve Endpoint bilgilerini kullanarak network kurallarını oluşturur. Bu kurallar, hangi isteklerin hangi Pod’a yönlendirilmesi gerektiğini belirler.
    - **Traffic Routing (Trafik Yönlendirme):**
        - Kube-proxy, dış dünyadan gelen trafiği doğru Pod’lara yönlendirir. Bu yönlendirme, IP tabanlı veya Layer 7 tabanlı (örn. HTTP) olabilir.
    - **Load Balancing (Yük Dengeleme):**
        - Kube-proxy, bir Service’in arkasındaki Pod’lar arasında yük dengelemesi yapar. İstekler, uygun Pod’lara dengeli bir şekilde dağıtılır.
3. **Kube-proxy Çalışma Modları:**
    - **Userspace Mode:**
        - Kube-proxy’nin en eski çalışma modudur. Bu modda, kube-proxy gelen istekleri kullanıcı alanında (userspace) alır ve uygun Pod’a yönlendirir. Performansı düşüktür ve modern Kubernetes kurulumlarında genellikle kullanılmaz.
    - **iptables Mode:**
        - Kube-proxy, iptables kurallarını kullanarak gelen trafiği Pod’lara yönlendirir. Bu mod, daha performanslıdır ve çoğu Kubernetes kurulumu için varsayılan moddur.
        - **Nasıl Çalışır:** Kube-proxy, Service ve Endpoint bilgilerini alır ve bu bilgilere göre iptables kuralları oluşturur. Bu kurallar, her node üzerinde çalışır ve gelen istekleri doğru Pod’a yönlendirir.
    - **IPVS Mode:**
        - Kube-proxy’nin en yeni çalışma modudur. IPVS (IP Virtual Server), Linux kernel modunda çalışır ve yük dengeleme işlemlerini daha verimli bir şekilde yapar.
        - **Nasıl Çalışır:** IPVS, iptables’e benzer ancak daha performanslıdır ve büyük ölçekli cluster’lar için uygundur. IPVS, binlerce Service’i ve Pod’u etkin bir şekilde yönetebilir.
4. **Kube-proxy’nin Yapı Taşları:**
    - **Service:**
        - Kube-proxy, Kubernetes Service objelerini kullanarak network trafiğini yönlendirir. Service, bir IP adresi ve bir dizi Pod’u temsil eder.
    - **Endpoints:**
        - Service’e ait olan Pod’ların IP adresleri ve portlarıdır. Kube-proxy, bu endpoint bilgilerini kullanarak trafiği yönlendirir.
    - **Network Rules:**
        - Kube-proxy, iptables veya IPVS kullanarak network kuralları oluşturur. Bu kurallar, gelen trafiğin nasıl yönlendirileceğini belirler.
    - **Health Checks:**
        - Kube-proxy, Pod’ların sağlık durumunu izler ve sadece sağlıklı Pod’lara trafik yönlendirir.
5. **Kube-proxy Yapılandırması:**
    - **Komut Satırı Bayrakları:**
        - `-proxy-mode`: Kube-proxy’nin çalışma modunu belirler (`userspace`, `iptables`, `ipvs`).
        - `-cluster-cidr`: Cluster içerisindeki Pod network CIDR bloğunu belirtir.
        - `-masquerade-all`: Tüm outbound trafiğin masquerade edilip edilmeyeceğini belirler.
        - `-healthz-bind-address`: Kube-proxy’nin sağlık kontrolü için kullanacağı adresi belirtir.
        - `-hostname-override`: Kube-proxy’nin node adı yerine kullanacağı hostname’i belirtir.
    - **ConfigMap Ayarları:**
        - Kube-proxy, yapılandırmasını bir ConfigMap üzerinden alabilir. Bu ConfigMap, kube-proxy’n in hangi modda çalışacağı ve diğer yapılandırma ayarlarını içerir.
        - Örnek ConfigMap:

        ```yaml
        apiVersion: v1
        kind: ConfigMap
        metadata:
        name: kube-proxy
        namespace: kube-system
        data:
        config.conf: |
        apiVersion: kubeproxy.config.k8s.io/v1alpha1
        kind: KubeProxyConfiguration
        mode: "ipvs"
        clusterCIDR: "10.244.0.0/16"
        ```

6. **Monitoring ve Loglama:**
    - **Kube-proxy Logları:**
        - Kube-proxy logları, ağ trafiğiyle ilgili sorunları izlemek ve çözmek için kullanılır. Loglar genellikle `/var/log/kube-proxy.log` dosyasında bulunur.
        - `journalctl -u kube-proxy` komutu ile kube-proxy loglarına erişebilirsiniz.
    - **Metrikler:**
        - Kube-proxy, çeşitli metrikleri Prometheus gibi izleme araçları ile raporlayabilir. Bu metrikler, network trafiğinin durumu ve performansı hakkında bilgi verir.
        - **Örnek Metrikler:**
            - `kubeproxy_sync_proxy_rules_duration_seconds`: Proxy kurallarının senkronizasyon süresi.
            - `kubeproxy_network_programming_latency_seconds`: Ağ programlama gecikmesi.
7. **Sık Karşılaşılan Sorunlar ve Çözümler:**
    - **Service’e Ulaşamama:**
        - **Sorun:** Pod’lar, belirli bir Service’e ulaşamıyor.
        - **Çözüm:** Kube-proxy’nin çalışıp çalışmadığını, iptables veya IPVS kurallarını kontrol edin. `kubectl describe svc <service-name>` komutuyla Service ve Endpoints bilgilerini doğrulayın.
    - **Yavaş Trafik Yönlendirmesi:**
        - **Sorun:** Ağ trafiği yavaş veya düzensiz şekilde yönlendiriliyor.
        - **Çözüm:** Kube-proxy’nin iptables veya IPVS modunda çalışıp çalışmadığını kontrol edin. IPVS, büyük ölçekli cluster’lar için daha performanslı olabilir.
    - **Network Kurallarıyla İlgili Sorunlar:**
        - **Sorun:** Yanlış yönlendirme veya erişim sorunları.
        - **Çözüm:** `iptables -L -t nat` komutu ile iptables kurallarını, `ipvsadm -Ln` komutu ile IPVS kurallarını kontrol edin. Kube-proxy’nin doğru çalıştığından emin olun.
8. **En İyi Uygulamalar:**
    - **Uygun Çalışma Modunu Seçin:** Cluster boyutunuza ve ihtiyaçlarınıza göre kube-proxy’nin çalışma modunu seçin. Küçük cluster’lar için iptables, büyük cluster’lar için IPVS önerilir.
    - **Monitoring’i Etkinleştirin:** Kube-proxy’nin metriklerini izleyin ve anormal durumlar için uyarılar oluşturun. Bu, ağ trafiğiyle ilgili sorunları erken tespit etmenize yardımcı olur.
    - **Düzenli Güncellemeler:** Kube-proxy ve bağlı olduğu container runtime’ı düzenli olarak güncelleyin, güvenlik ve performans sorunlarını minimize edin.
    - **Network Politikalarını Kullanın:** Ağ güvenliğini artırmak için network policy’leri kullanarak kube-proxy tarafından yönetilen trafiği kontrol edin.

## Kubeadm

1. **Kubeadm Nedir?**
    - **Tanım:** Kubeadm, Kubernetes cluster'ları kolayca kurmak, yapılandırmak ve yönetmek için kullanılan bir araçtır. Kubeadm, Kubernetes’in en temel bileşenlerini hızlıca devreye alarak çalışan bir cluster oluşturmanıza yardımcı olur.
    - **Görev:** Kubeadm, cluster oluşturma ve yönetim işlemlerini otomatikleştirir, böylece Kubernetes cluster’larını hızlı ve güvenilir bir şekilde kurmanıza olanak tanır.
2. **Kubeadm’in Temel Görevleri:**
    - **Cluster Initialization (Cluster Başlatma):**
        - `kubeadm init` komutuyla Kubernetes kontrol düzlemini (control plane) başlatır ve master node’u kurar. Bu süreç, etcd, API Server, Controller Manager ve Scheduler gibi temel bileşenleri içerir.
    - **Node Join (Node Katılımı):**
        - `kubeadm join` komutuyla yeni worker node’ları mevcut cluster’a katabilirsiniz. Kubeadm, gerekli yapılandırma ve sertifika işlemlerini otomatik olarak yapar.
    - **Cluster Yönetimi:**
        - Kubeadm, cluster’ın güncellenmesi, genişletilmesi, yedeklenmesi ve geri yüklenmesi gibi yönetimsel görevleri de destekler.
    - **Sertifika Yönetimi:**
        - Kubeadm, Kubernetes bileşenleri için gerekli olan sertifikaları otomatik olarak oluşturur ve yönetir. `kubeadm certs` komutuyla sertifika yenileme işlemleri yapılabilir.
3. **Kubeadm’in Kullanım Aşamaları:**
    - **1. Aşama: Kurulum**
        - Kubeadm ve gerekli bağımlılıkları (kubelet, kubectl) yükleyin. Bu adım, genellikle paket yöneticileri (örn. apt, yum) ile gerçekleştirilir.
        - Örnek Kurulum Komutları (Ubuntu):

        ```bash
        sudo apt-get update && sudo apt-get install -y kubeadm kubelet kubectl
        sudo apt-mark hold kubeadm kubelet kubectl
        ```

    - **2. Aşama: Cluster Başlatma**
        - **Control Plane Başlatma:**
            - `kubeadm init` komutunu çalıştırarak control plane bileşenlerini başlatın. Bu komut, API Server, etcd, Controller Manager ve Scheduler gibi temel bileşenleri başlatır.
            - Örnek Komut:

            ```bash
            sudo kubeadm init --pod-network-cidr=10.244.0.0/16
            ```

            - Komut tamamlandıktan sonra, kubeconfig dosyasını ayarlayın:

            ```bash
            mkdir -p $HOME/.kube
            sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
            sudo chown $(id -u):$(id -g) $HOME/.kue/config
            ```

        - **Pod Network Ekleme:**
            - Cluster’ınızın Pod network’ünü ayarlamak için bir CNI (Container Network Interface) eklentisi kurun. Örneğin, Flannel:

            ```bash
            kubectl apply -f <https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml>
            ```

    - **3. Aşama: Node Katılımı**
        - Worker node’ları cluster’a eklemek için `kubeadm join` komutunu çalıştırın. Bu komutu, control plane başlatma sırasında elde edilen token ve API Server bilgileriyle çalıştırabilirsiniz.
        - Örnek Komut:

        ```bash
        sudo kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
        ```

4. **Kubeadm Komutları ve Kullanımı:**
    - **Kubeadm Init:**
        - Cluster’ı başlatmak ve master node’u kurmak için kullanılır.
        - Örnek: `kubeadm init --pod-network-cidr=10.244.0.0/16`
    - **Kubeadm Join:**
        - Worker node’ları mevcut bir cluster’a eklemek için kullanılır.
        - Örnek: `kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>`
    - **Kubeadm Reset:**
        - Cluster kurulumu sırasında yapılan tüm değişiklikleri geri almak için kullanılır. Bu komut, bir node’u yeniden kuruluma hazırlamak için kullanılır.
        - Örnek: `kubeadm reset`
    - **Kubeadm Upgrade:**
        - Mevcut bir Kubernetes cluster’ını yükseltmek için kullanılır.
        - Örnek: `kubeadm upgrade plan` ve ardından `kubeadm upgrade apply <version>`
    - **Kubeadm Token:**
        - Yeni node’lar eklemek için kullanılacak token’ları oluşturmak ve yönetmek için kullanılır.
        - Örnek: `kubeadm token create`
    - **Kubeadm Certs:**
        - Sertifika yenileme işlemleri için kullanılır. Sertifikaların durumunu kontrol etmek ve gerektiğinde yenilemek için bu komut kullanılır.
        - Örnek: `kubeadm certs renew all`
5. **Kubeadm Kullanımında Dikkat Edilmesi Gerekenler:**
    - **DNS Ayarları:**
        - Kubernetes cluster’ı için uygun DNS çözümlemesinin sağlandığından emin olun. Kubeadm, cluster içerisinde DNS işlevselliği için CoreDNS gibi eklentileri kullanır.
    - **Zaman Senkronizasyonu:**
        - Tüm node’ların doğru saat ve tarih ayarlarına sahip olduğundan emin olun. Zaman senkronizasyonu, özellikle sertifikalar ve token’lar için önemlidir.
    - **Swap Kapalı Olmalı:**
        - Kubeadm, Kubernetes node’larında swap’in devre dışı olmasını gerektirir. Kurulumdan önce swap’i kapattığınızdan emin olun.
        - Örnek Komut:

        ```bash
        sudo swapoff -a
        ```

6. **Monitoring ve Loglama:**
    - **Kubeadm Logları:**
        - Kubeadm komutlarının logları, kurulum sırasında meydana gelen sorunları çözmek için kullanılabilir. Loglar, genellikle `journalctl` veya ilgili dosyalarda bulunabilir.
        - Örnek Komut:

        ```bash
        journalctl -xeu kubeadm
        ```

    - **Kubeadm Cluster Durumu:**
        - `kubectl get nodes` komutuyla cluster’daki tüm node’ların durumunu kontrol edebilirsiniz.
        - Control plane bileşenlerinin durumunu `kubectl get pods -n kube-system` komutuyla izleyebilirsiniz.
7. **Sık Karşılaşılan Sorunlar ve Çözümler:**
    - **Control Plane Erişim Sorunları:**
        - **Sorun:** Worker node’lar, control plane’e katılamıyor.
        - **Çözüm:** API Server’ın doğru IP ve portta çalıştığını doğrulayın. Firewall ayarlarını kontrol edin ve gerekli portların açık olduğundan emin olun.
    - **DNS Çözümleme Sorunları:**
        - **Sorun:** Pod’lar, DNS hizmetlerine erişemiyor.
        - **Çözüm:** CoreDNS veya diğer DNS eklentilerinin doğru çalışıp çalışmadığını kontrol edin. `kubectl logs -n kube-system -l k8s-app=kube-dns` komutuyla DNS pod’larının loglarını inceleyin.
    - **Sertifika Geçerlilik Sorunları:**
        - **Sorun:** Sertifikalar geçersiz veya süresi dolmuş.
        - **Çözüm:** `kubeadm certs renew all` komutuyla sertifikaları yenileyin ve kubelet hizmetini yeniden başlatın.
8. **En İyi Uygulamalar:**
    - **Yedekleme ve Geri Yükleme:** Control plane bileşenlerinin ve etcd veritabanının düzenli yedeklerini alın. Yedekler, olası bir başarısızlık durumunda cluster’ın geri yüklenmesini sağlar.
    - **Güncellemeleri Planlayın:** Kubeadm kullanarak Kubernetes cluster’ınızı düzenli olarak güncelleyin. Güncellemeler, güvenlik yamaları ve yeni özellikler içerir.
    - **Güvenlik:** Sertifikaları düzenli olarak yenileyin ve Kubernetes API Server erişimini güvenlik duvarlarıyla koruyun. Ayrıca, RBAC politikalarını kullanarak yetkilendirmeyi yönetin.
    - **Cluster Yönetimi:** `kubectl` ve `kubeadm` komutlarını birleştirerek cluster yönetimini otomatikleştirin. Örneğin, kubeadm ile yeni node’lar eklerken, kubectl ile bu node’ların sağlığını izleyin.

## Pod

1. **Pod Nedir?**
    - **Tanım:** Pod, Kubernetes'teki en küçük dağıtım birimidir. Bir veya birden fazla container'ı gruplar ve bu container'lar aynı node üzerinde çalışır. Container'lar bir Pod içinde birlikte çalışır, ağ ve depolama kaynaklarını paylaşır.
    - **Görev:** Pod, container'ları birlikte çalışacak şekilde organize eder ve yönetir. Kubernetes, uygulamaları Pod'lar üzerinden dağıtır, yönetir ve ölçeklendirir.
2. **Pod’un Temel Özellikleri:**
    - **Tek veya Çoklu Container:**
        - **Tek Container:** Çoğu durumda bir Pod, tek bir container içerir ve bu container’ın yaşam döngüsünü yönetir.
        - **Çoklu Container:** Bir Pod, birbiriyle sıkı bir şekilde bağlı birden fazla container içerebilir. Bu container’lar, aynı node üzerinde, aynı network namespace'i paylaşarak çalışır.
    - **Shared Network:**
        - Pod içindeki tüm container’lar, aynı IP adresini ve port alanını paylaşır. Bu sayede, container’lar birbirleriyle `localhost` üzerinden iletişim kurabilir.
    - **Shared Storage:**
        - Pod içindeki container’lar, aynı volume’leri paylaşabilir. Bu, container’lar arasında veri paylaşımını sağlar.
3. **Pod Yapı Taşları:**
    - **PodSpec:**
        - Pod’un nasıl çalışacağını tanımlayan yapılandırmadır. İçerdiği bileşenler arasında container tanımları, volume’ler, network ayarları, ve yaşam döngüsü yönetimi gibi özellikler bulunur.
    - **Container:**
        - Pod’un çalıştıracağı bir veya daha fazla container tanımlanır. Her container için image, environment variables, command, args, ports gibi parametreler belirtilir.
    - **Volume:**
        - Pod içindeki container’lar arasında veri paylaşımı için kullanılan depolama alanlarıdır. Örneğin, bir volume, bir container’da bir log dosyasını yazarken, diğer container bu dosyayı okuyabilir.
    - **Labels ve Annotations:**
        - Pod’ları organize etmek ve yönetmek için kullanılan metadata’dır. Labels, Pod’ları seçmek ve gruplamak için kullanılırken, annotations Pod hakkında ek bilgi sağlamak için kullanılır.
    - **Resource Requests and Limits:**
        - Her container’ın kullanabileceği CPU ve bellek kaynakları belirtilir. Requests, minimum garanti edilen kaynak miktarını belirtir; limits ise maksimum kullanılabilir kaynak miktarını tanımlar.
4. **Pod Yaşam Döngüsü:**
    - **Pending:**
        - Pod, Kubernetes tarafından planlanmış (scheduled) ancak henüz çalıştırılmamış (container'lar başlatılmamış) durumdadır. Bu durumda genellikle kaynak beklenmektedir.
    - **Running:**
        - Pod çalışır durumdadır ve en az bir container'ı başarılı bir şekilde başlatılmıştır.
    - **Succeeded:**
        - Pod içindeki tüm container'lar başarıyla tamamlanmıştır ve tekrar başlatılmayacaktır. Bu durum genellikle tek seferlik işler (jobs) için geçerlidir.
    - **Failed:**
        - Pod içindeki en az bir container, başarısız olmuş ve tüm container'lar sonlandırılmıştır.
    - **CrashLoopBackOff:**
        - Pod, sürekli olarak çöküyor ve Kubernetes, Pod’u belirli aralıklarla yeniden başlatmaya çalışıyor.
5. **Pod Türleri:**
    - **Standalone Pods (Bağımsız Pod'lar):**
        - Tek başına çalışan Pod’lar. Genellikle test ve geliştirme ortamlarında kullanılır. Kubernetes, bu tür Pod'ları doğrudan yönetmez; bu nedenle, Pod başarısız olursa yeniden başlatılmaz.
    - **Replication Controller/ReplicaSet Pod'ları:**
        - Bu Pod’lar bir Replication Controller veya ReplicaSet tarafından yönetilir. Belirtilen sayıda Pod’un her zaman çalışır durumda olmasını sağlar.
    - **DaemonSet Pod'ları:**
        - Cluster’daki her node’da bir Pod’un çalışmasını sağlar. Bu tür Pod’lar genellikle sistem izleme, log toplama gibi görevler için kullanılır.
    - **Job/CronJob Pod'ları:**
        - Belirli bir işi gerçekleştirmek için tek seferlik çalışan Pod’lar. Job, tek seferlik işler için; CronJob ise belirli zaman aralıklarında tekrarlanan işler için kullanılır.
6. **Pod Yapılandırma:**
    - **Pod Tanımı (YAML):**
        - Pod'lar, genellikle YAML formatında tanımlanır. Bu tanımda Pod’un metadata’sı, spec’i (container, volume, vs.) ve diğer özellikleri belirtilir.
        - Örnek Pod Tanımı:

        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
        name: my-pod
        labels:
            app: my-app
        spec:
        containers:
        - name: my-container
            image: nginx:latest
            ports:
            - containerPort: 80
         ```

    - **Environment Variables:**
       - Container’lara environment variable’lar atanabilir. Bu değişkenler, Pod çalıştırıldığında container içinde kullanılabilir.
    - **Init Containers:**
       - Bir Pod, init container’ları içerebilir. Init container’lar, ana container’lar başlamadan önce belirli görevleri yerine getiren container’lardır. Örneğin, bir init container, bir yapılandırma dosyasını indirip hazırlayabilir.
    - **Probes:**
       - Kubernetes, Pod’ların sağlık durumunu kontrol etmek için liveness ve readiness probes kullanır. Liveness probe, bir container’ın canlı olup olmadığını; readiness probe ise bir container’ın trafiği alabilecek durumda olup olmadığını kontrol eder.

7. **Pod Yönetimi:**
    - **Pod Oluşturma:**
        - `kubectl apply -f <yaml-dosyası>` komutuyla bir Pod oluşturabilirsiniz. Bu komut, Pod tanımını API Server’a gönderir ve Pod’u çalıştırır.
    - **Pod Durumunu Görüntüleme:**
        - `kubectl get pods` komutuyla cluster’daki tüm Pod’ların durumunu görebilirsiniz.
    - **Pod Detaylarını İnceleme:**
        - `kubectl describe pod <pod-adı>` komutuyla bir Pod hakkında ayrıntılı bilgi alabilirsiniz. Bu bilgi, Pod’un çalışıp çalışmadığını ve varsa hataları içerir.
    - **Pod Loglarını Görüntüleme:**
        - `kubectl logs <pod-adı>` komutuyla bir Pod’un loglarını inceleyebilirsiniz.
    - **Pod Silme:**
        - `kubectl delete pod <pod-adı>` komutuyla bir Pod’u silebilirsiniz. Bu, Pod’u ve içindeki container’ları durdurur ve kaldırır.
8. **Sık Karşılaşılan Sorunlar ve Çözümler:**
    - **ImagePullBackOff:**
        - **Sorun:** Pod, container image’ını çekemiyor.
        - **Çözüm:** Image adı, sürümü ve image registry bağlantısını kontrol edin. Gerekirse image çekme yetkisini kontrol edin (örn. Docker Hub, private registry).
    - **CrashLoopBackOff:**
        - **Sorun:** Pod, sürekli olarak çöküyor ve Kubernetes yeniden başlatmaya çalışıyor.
        - **Çözüm:** Pod loglarını inceleyin (`kubectl logs <pod-adı>`). Genellikle, container’da bir hata veya yanlış yapılandırma bu duruma neden olur.
    - **Pod Not Ready:**
        - **Sorun:** Pod, çalışıyor ama trafiği kabul etmeye hazır değil.
        - **Çözüm:** Readiness probe ayarlarını kontrol edin. Container, gerekli tüm başlangıç işlemlerini tamamladığından emin olun.
9. **En İyi Uygulamalar:**
    - **Resource Requests and Limits Kullanın:** Pod’lar için uygun CPU ve bellek talepleri ve limitleri belirleyin. Bu, kaynakların adil dağıtılmasını sağlar ve Pod’ların birbirini etkilemesini önler.
    - **Probes Kullanın:** Pod’ların sağlık durumunu izlemek ve trafiği kabul etmeye hazır olup olmadıklarını kontrol etmek için liveness ve readiness probes kullanın.
    - **Init Containers Kullanın:** Ana container’lar başlamadan önce gerekli hazırlıkları yapmak için init container’ları kullanın.
    - **Labels ve Annotations Kullanın:** Pod’ları organize etmek ve yönetmek için etkili bir şekilde labels ve annotations kullanın. Bu, Pod’ların seçilmesini ve izlenmesini kolaylaştırır.
    - **Pod’ları StatefulSet veya Deployment ile Yönetin:** Pod’ları doğrudan yönetmek yerine, uzun ömürlü Pod’lar için StatefulSet, stateless Pod’lar için Deployment kullanın. Bu, Pod yönetimini ve ölçeklenmesini kolaylaştırır.

## YAML'in Kubernetes ile İlişkisi

1. **Kubernetes'te YAML Kullanımı:**
    - Kubernetes, kaynakların tanımını ve yapılandırmasını yapmak için YAML dosyalarını kullanır. Yani, Kubernetes’teki her şeyin (Pod, Service, Deployment, vb.) nasıl çalışacağını YAML dosyalarıyla belirliyoruz.
    - YAML, Kubernetes ile olan ilişkisi sayesinde, bir cluster'da neyin, nasıl ve nerede çalışacağını tanımlayan bir tür "talimat belgesi" görevi görüyor.
2. **YAML ile Kubernetes Kaynaklarını Tanımlama:**
    - **Pod Tanımları:**
        - Bir Pod’un nasıl çalışacağını, hangi container’ları içereceğini, container’ların hangi image’leri kullanacağını ve hangi portlarda çalışacağını YAML ile tanımlıyoruz.
        - Örnek:

        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
        name: my-pod
        spec:
        containers:
        - name: my-container
            image: nginx:latest
            ports:
            - containerPort: 80
        ```

        - Bu örnekte, `my-pod` adında bir Pod oluşturuluyor ve içinde `nginx:latest` image’iyle çalışan bir container var. Bu YAML dosyası, Kubernetes'e bu container’ın nasıl çalışması gerektiğini anlatıyor.
    - **Service Tanımları:**
        - Pod’ları dış dünyaya veya cluster içindeki diğer bileşenlere erişilebilir hale getirmek için Service tanımları kullanıyoruz. Bu tanımlar da YAML dosyalarıyla yapılır.
        - Örnek:

        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: my-service
        spec:
          selector:
            app: my-app
          ports:

        - protocol: TCP
           port: 80
           targetPort: 80
        ```

        - Bu YAML dosyası, `my-service` adında bir Service oluşturuyor ve `my-app` etiketine sahip Pod’ları hedefliyor. Bu sayede, bu Pod’lar dışarıdan TCP 80 portu üzerinden erişilebilir hale geliyor.
    - **Deployment Tanımları:**
        - Bir uygulamanın nasıl dağıtılacağını ve yönetileceğini Deployment tanımları ile belirliyoruz. Deployment, belirli sayıda Pod’un her zaman çalışır durumda olmasını sağlar.
        - Örnek:

        ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: my-deployment
        spec:
          replicas: 3
          selector:
            matchLabels:
              app: my-app
          template:
            metadata:
              labels:
                app: my-app
            spec:
              containers:
              - name: my-container
                image: nginx:latest
                ports:
                - containerPort: 80
        ```

        - Bu YAML dosyasında, `my-deployment` adında bir Deployment tanımlanıyor ve her zaman 3 Pod’un çalışır durumda olmasını sağlıyor. Bu Pod’lar da yine `nginx:latest` image’iyle çalışıyor.

3. **Kubernetes'in YAML Dosyalarını Kullanması:**
    - Kubernetes, bu YAML dosyalarını okur, tanımlarını değerlendirir ve ilgili kaynakları (Pod, Service, Deployment vb.) cluster'da oluşturur veya günceller.
    - **Uygulama:**
        - `kubectl apply -f my-pod.yaml` gibi bir komutla, YAML dosyasındaki tanımları cluster’da uygulayabiliriz. Bu komut, Kubernetes’e "bu Pod’u oluştur ve çalıştır" talimatını verir.
    - **Güncelleme:**
        - YAML dosyasındaki tanımları değiştirip tekrar uyguladığımızda (`kubectl apply -f my-pod.yaml`), Kubernetes bu değişiklikleri mevcut kaynaklara uygular.
    - **Görüntüleme:**
        - `kubectl get pod my-pod -o yaml` komutuyla, mevcut bir Pod’un tanımını YAML formatında görebiliriz. Bu, mevcut durumun nasıl tanımlandığını anlamak için kullanışlıdır.
4. **Neden YAML?**
    - **İnsan Okunabilirliği:** YAML, insan tarafından okunabilir ve yazılabilir. Bu, Kubernetes gibi karmaşık bir sistemi yönetirken büyük avantaj sağlıyor.
    - **Versiyon Kontrolü:** YAML dosyaları, kolayca versiyon kontrol sistemlerinde saklanabilir. Bu, yapılandırma değişikliklerini izlemeyi ve gerektiğinde geri almayı kolaylaştırır.
    - **Deklaratif Yaklaşım:** Kubernetes’te YAML kullanarak kaynakları tanımlamak, deklaratif bir yaklaşımı benimsememizi sağlar. Biz neyin, nasıl olması gerektiğini söyleriz, Kubernetes ise bunları bizim için uygular.
5. **Sonuç:**
    - YAML, Kubernetes ile doğrudan ilişkilidir ve Kubernetes kaynaklarını tanımlamak, yönetmek ve güncellemek için en önemli araçtır. Bu dosyalar, Kubernetes’in çalışması için gerekli olan tüm yapılandırmaları içerir ve sistemin nasıl çalışacağını belirler.
    - Dersin en can alıcı noktası şu: Kubernetes’te her şey YAML ile başlar. İyi bir Kubernetes yöneticisi olmanın yolu, YAML’i anlamaktan ve etkili bir şekilde kullanabilmekten geçiyor.

## Pod Definition YAML Örnekleri ve Olası Alan Seçenekleri

Aşağıda, bir Pod tanımının YAML formatında nasıl göründüğünü ve bu tanım içindeki alanların alabileceği olası değerleri gösteren örnekler ve tablolar yer alıyor.

#### Örnek Pod Tanımı (YAML)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
  labels:
    app: example
spec:
  containers:
  - name: example-container
    image: nginx:latest
    ports:
    - containerPort: 80
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    env:
    - name: ENV_VAR_NAME
      value: "env_value"
    volumeMounts:
    - name: example-volume
      mountPath: /usr/share/nginx/html
  volumes:
  - name: example-volume
    emptyDir: {}
  restartPolicy: Always
```

#### Pod Tanımı İçindeki Alanlar ve Olası Seçenekler

| **Alan**        | **Açıklama**                                                                                                          | **Olası Seçenekler / Değerler**                                                                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiVersion`    | Kubernetes API sürümünü belirtir.                                                                                     | - `v1` (core resources)  <br>- `apps/v1`, `batch/v1`, `extensions/v1beta1`, vb. (diğer kaynaklar için)                                                                |
| `kind`          | Kaynak türünü belirtir.                                                                                               | - `Pod`                                                                                                                                                               |
| `metadata`      | Kaynağın adı, etiketleri ve diğer meta bilgileri.                                                                     | - `name`: Pod adı (örneğin, `example-pod`)  <br>- `labels`: Pod’u etiketlemek için key-value çiftleri (örn. `app: example`)                                           |
| `spec`          | Pod’un çalışma koşullarını tanımlar.                                                                                  | - `containers`: Pod içinde çalışacak container'ları tanımlar. (Aşağıda ayrıntılı tablo mevcut)  <br>- `volumes`: Pod'a ait volume tanımları                           |
| `containers`    | Pod içindeki container'ları tanımlar. Her bir container için ayrı bir `name`, `image` ve diğer özellikler belirtilir. | - `name`: Container adı (örn. `example-container`)  <br>- `image`: Container image’ı (örn. `nginx:latest`)  <br>- Diğer alanlar için aşağıdaki tabloya bakınız.       |
| `restartPolicy` | Pod’un container’ları ne zaman yeniden başlatacağını belirtir.                                                        | - `Always`: Container her zaman yeniden başlatılır.  <br>- `OnFailure`: Container başarısız olursa yeniden başlatılır.  <br>- `Never`: Container yeniden başlatılmaz. |
| `volumes`       | Pod'a ait volume tanımları. Container'lar bu volume'leri `volumeMounts` ile bağlarlar.                                | - `name`: Volume adı (örn. `example-volume`)  <br>- Volume türleri: `emptyDir`, `hostPath`, `persistentVolumeClaim`, `configMap`, vb.                                 |
| `volumeMounts`  | Container içinde volume’lerin mount edilmesi için kullanılır.                                                         | - `name`: Bağlanacak volume adı  <br>- `mountPath`: Volume’ün container içinde mount edileceği yol (örn. `/usr/share/nginx/html`)                                     |

#### `containers` Alanındaki Olası Seçenekler

| **Alt Alan**      | **Açıklama**                                                          | **Olası Seçenekler / Değerler**                                                                                                                                   |
| ----------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`            | Container’ın adı.                                                     | Örnek: `example-container`                                                                                                                                        |
| `image`           | Container’ın çalışacağı image.                                        | Örnek: `nginx:latest`, `alpine:3.12`                                                                                                                              |
| `ports`           | Container’da açılacak portları belirtir.                              | - `containerPort`: Container’da açılacak port numarası (örn. `80`)                                                                                                |
| `env`             | Container için environment variables tanımlar.                        | - `name`: Değişken adı (örn. `ENV_VAR_NAME`)  <br>- `value`: Değişken değeri (örn. `env_value`)                                                                   |
| `resources`       | Container’ın kullanabileceği kaynakları tanımlar.                     | - `requests`: Minimum garanti edilen kaynaklar (örn. `memory: "64Mi"`, `cpu: "250m"`)  <br>- `limits`: Maksimum kullanılabilir kaynaklar (örn. `memory: "128Mi"`) |
| `command`         | Container başlatıldığında çalıştırılacak komutlar.                    | Örnek: `["/bin/sh", "-c", "echo Hello Kubernetes!"]`                                                                                                              |
| `args`            | `command` ile birlikte çalıştırılacak argümanlar.                     | Örnek: `["-c", "echo", "Hello Kubernetes!"]`                                                                                                                      |
| `volumeMounts`    | Container içindeki dosya sistemine volume’leri mount eder.            | - `name`: Volume adı  <br>- `mountPath`: Container içinde mount edileceği yol (örn. `/usr/share/nginx/html`)                                                      |
| `livenessProbe`   | Container’ın canlı olup olmadığını kontrol eder.                      | - `httpGet`, `tcpSocket`, `exec` seçenekleri  <br>- Örnek: `httpGet: path: /healthz, port: 8080`                                                                  |
| `readinessProbe`  | Container’ın trafiği kabul etmeye hazır olup olmadığını kontrol eder. | - `httpGet`, `tcpSocket`, `exec` seçenekleri  <br>- Örnek: `httpGet: path: /readyz, port: 8080`                                                                   |
| `imagePullPolicy` | Image’in nasıl çekileceğini belirtir.                                 | - `Always`: Her zaman image’i çek  <br>- `IfNotPresent`: Eğer image localde yoksa çek  <br>- `Never`: Hiçbir zaman image’i çekme                                  |
| `securityContext` | Container için güvenlik ayarları yapar.                               | - `runAsUser`: Container’ı çalıştıracak kullanıcının UID’si  <br>- `privileged`: Container’ı ayrıcalıklı modda çalıştırır (örn. `true`)                           |

Bu tablolar, Kubernetes Pod tanımları için en yaygın kullanılan alanları ve olası seçenekleri kapsamaktadır. Kubernetes YAML dosyalarını hazırlarken bu alanları ve seçenekleri doğru bir şekilde kullanmak, sistemin istenilen şekilde çalışmasını sağlar.

## Labels ve Selectors

1. **Labels Nedir?**
    - **Tanım:** Labels, Kubernetes’teki nesnelere (Pod, Service, ReplicaSet, Deployment, vb.) eklenebilen key-value çiftleridir. Labels, nesneleri kategorize etmek, organize etmek ve belirli gruplar altında toplamak için kullanılır.
    - **Görev:** Labels, Kubernetes nesnelerini kolayca seçebilmek, filtreleyebilmek ve gruplayabilmek için esnek ve kullanışlı bir yol sağlar. Uygulama bileşenlerinin yönetiminde, nesneleri sınıflandırmak için yaygın olarak kullanılır.
2. **Labels Kullanımı:**
    - **Uygulama Bileşenlerini Gruplama:**
        - Farklı bileşenlere sahip bir uygulamayı yönetmek için labels kullanılabilir. Örneğin, `app=frontend`, `app=backend`, `env=production` gibi etiketler.
    - **Çoklu Versiyonları Yönetme:**
        - Uygulamanın farklı versiyonlarını yönetmek için kullanılabilir. Örneğin, `version=v1`, `version=v2` gibi etiketler.
    - **Ortam Ayırımı:**
        - Farklı ortamları ayırmak için labels kullanılabilir. Örneğin, `env=production`, `env=staging`, `env=development` gibi.
3. **Label Örnekleri:**

```yaml
metadata:
  name: example-pod
  labels:
    app: webserver
    env: production
    version: v1
```

- Bu örnekte, `example-pod` adlı Pod’a `app=webserver`, `env=production`, ve `version=v1` etiketleri eklenmiştir.

    **Labels için Örnek Kullanım Alanları**

| **Label Key** | **Label Value**                        | **Kullanım Alanı**                                                       |
| ------------- | -------------------------------------- | ------------------------------------------------------------------------ |
| `app`         | `frontend`, `backend`, `database`      | Uygulamanın bileşenlerini ayırmak için kullanılır.                       |
| `env`         | `production`, `staging`, `development` | Farklı ortamları ayırmak için kullanılır.                                |
| `tier`        | `frontend`, `backend`, `database`      | Uygulamanın hangi katmanına ait olduğunu belirtir.                       |
| `version`     | `v1`, `v2`, `latest`                   | Uygulamanın versiyonlarını belirtir.                                     |
| `release`     | `canary`, `stable`                     | Yayın türlerini belirtmek için kullanılır (örneğin, `canary`, `stable`). |

1. **Selectors Nedir?**
    - **Tanım:** Selectors, belirli bir label setine sahip Kubernetes nesnelerini seçmek ve yönetmek için kullanılan sorgulardır. Selectors, bir Service, ReplicaSet veya Deployment’ın hangi Pod’ları seçeceğini belirlemek için kullanılır.
    - **Görev:** Selectors, belirli bir label kombinasyonuna sahip olan nesneleri hedef alarak, bu nesneler üzerinde işlemler yapmanıza olanak tanır. Örneğin, bir Service, belirli label’lara sahip Pod’ları hedefleyerek trafiği yönlendirebilir.
2. **Selectors Türleri:**
    - **Equality-Based Selectors (Eşitlik Tabanlı Seçiciler):**
        - Eşitlik tabanlı selectorda, bir label’in belirli bir değere sahip olup olmadığı sorgulanır.
        - Bu selector, `app=webserver` ve `env=production` etiketlerine sahip tüm Pod’ları seçecektir.

    ```yaml
    selector:
      matchLabels:
        app: webserver
        env: production
    ```

    - **Set-Based Selectors (Küme Tabanlı Seçiciler):**
        - Küme tabanlı selectorda, bir label’in belirli bir değer kümesinde olup olmadığı veya belirli değerlerin dışında olup olmadığı sorgulanır.
        - Örnek:

        ```yaml
        selector:
          matchExpressions:
          - key: env
            operator: In
            values: ["staging", "production"]
          - key: version
            operator: NotIn
            values: ["v1"]
        ```

        - Bu selector, `env` etiketi `staging` veya `production` olan ve `version` etiketi `v1` olmayan tüm nesneleri seçecektir.

#### Selectors için Örnek Kullanım Alanları

| **Selector Türü**  | **Örnek**                      | **Açıklama**                                                        |
| ------------------ | ------------------------------ | ------------------------------------------------------------------- |
| **Equality-Based** | `app=webserver`                | `app=webserver` label’ine sahip tüm Pod’ları seçer.                 |
| **Set-Based (In)** | `env In (staging, production)` | `env` etiketi `staging` veya `production` olan tüm nesneleri seçer. |

## Deployment

1. **Deployment Nedir?**
    - **Tanım:** Deployment, Kubernetes'te uygulamaların yönetimini ve dağıtımını kolaylaştıran yüksek seviyeli bir yapılandırma nesnesidir. Deployment, ReplicaSet ve Pod’ları yönetir, uygulamanın belirli sayıda Pod ile kesintisiz bir şekilde çalışmasını sağlar.
    - **Görev:** Deployment, uygulama sürümlerini dağıtmak, güncellemek, ölçeklendirmek ve geri almak gibi işlemleri yönetir. Deployment, uygulama güncellemelerini güvenli ve kesintisiz bir şekilde yapmayı kolaylaştırır.
2. **Deployment’ın Temel İşlevleri:**
    - **Otomatik Güncelleme ve Rollback:**
        - Deployment, Pod’ları güncellerken eski Pod’ları otomatik olarak kaldırır ve yeni Pod’ları başlatır. Eğer bir sorun yaşanırsa, önceki başarılı duruma geri dönebilir (rollback).
    - **Ölçeklendirme:**
        - Deployment, Pod sayısını artırarak veya azaltarak uygulamanın yükünü yönetir.
    - **Sürüm Yönetimi:**
        - Yeni bir uygulama sürümü dağıtıldığında, Deployment eski sürümü yavaş yavaş kaldırır ve yeni sürümü başlatır. Bu işlem, rollout olarak adlandırılır.
    - **Sağlık Kontrolü:**
        - Deployment, Pod’ların sağlığını izler ve sorunlu Pod’ları yeniden başlatır veya güncellemeleri geri alır.
3. **Deployment Tanımı (YAML):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
  labels:
    app: webserver
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      labels:
        app: webserver
    spec:
      containers:
      - name: nginx
        image: nginx:1.17
        ports:
        - containerPort: 80
```

#### Deployment Tanımı İçindeki Alanlar ve Olası Değerler

| **Alan**     | **Açıklama**                                                                    | **Olası Değerler / Seçenekler**                                                                                                                |
| ------------ | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiVersion` | Kubernetes API sürümünü belirtir.                                               | - `apps/v1`                                                                                                                                    |
| `kind`       | Kaynak türünü belirtir.                                                         | - `Deployment`                                                                                                                                 |
| `metadata`   | Deployment’a ait meta bilgileri içerir, örneğin ad ve etiketler.                | - `name`: Deployment adı (örn. `example-deployment`)  <br>- `labels`: Deployment için etiketler (örn. `app: webserver`)                        |
| `spec`       | Deployment’ın nasıl çalışacağını tanımlar.                                      | - `replicas`: Çalıştırılacak Pod sayısı (örn. `3`)  <br>- `selector`: Pod’ları seçmek için kullanılan etiketler  <br>- `template`: Pod şablonu |
| `replicas`   | Çalışır durumda olması istenen Pod sayısını belirtir.                           | - Sayısal değer (örn. `3`, `5`, `10`)                                                                                                          |
| `selector`   | Deployment’in hangi Pod’ları yöneteceğini belirlemek için kullanılan etiketler. | - `matchLabels`: Belirli etiketlere sahip Pod’ları seçmek için kullanılır (örn. `app: webserver`)                                              |
| `template`   | Yeni Pod'ların nasıl oluşturulacağını belirler (Pod şablonu).                   | - `metadata`: Pod için etiketler ve ad tanımları  <br>- `spec`: Pod’un container, volume, restart policy vb. özelliklerini içerir              |
| `containers` | Deployment’in yöneteceği Pod'lar içinde çalışacak container’ları tanımlar.      | - `name`: Container adı (örn. `nginx`)  <br>- `image`: Container image’ı (örn. `nginx:1.17`)  <br>- Diğer container alanları                   |

4. **Deployment'ın Sağladığı Avantajlar:**
    - **Kesintisiz Güncelleme (Rolling Updates):**
        - Deployment, uygulamanın yeni sürümünü dağıtırken eski Pod’ları kesintisiz bir şekilde yeni Pod’larla değiştirir. Bu, uygulama güncellemeleri sırasında hizmet kesintisini en aza indirir.
    - **Geri Alma (Rollback):**
        - Yeni bir dağıtımda hata oluşursa, Deployment eski sürüme geri dönerek sistemi güvenli bir duruma getirir.
    - **Deklaratif Yönetim:**
        - Deployment, istenen durumu (desired state) deklaratif olarak tanımlar ve Kubernetes bu durumu sağlamak için gerekli adımları otomatik olarak atar.
    - **Otomatik Ölçekleme:**
        - Pod sayısını dinamik olarak artırabilir veya azaltabilir. Bu, kaynak kullanımını optimize eder ve maliyetleri düşürür.
5. **Deployment Rollout ve Rollback:**
    - **Rollout:**
        - Yeni bir Deployment sürümünü uygulamak için kullanılır. Bu işlem sırasında, eski Pod’lar yavaş yavaş kaldırılır ve yerlerine yeni Pod’lar başlatılır.
        - **Komut:** `kubectl rollout status deployment/example-deployment` - Deployment’ın rollout durumunu kontrol eder.
    - **Rollback:**
        - Eğer yeni bir sürümde bir hata oluşursa, Deployment eski başarılı sürüme geri dönebilir.
        - **Komut:** `kubectl rollout undo deployment/example-deployment` - Deployment’ı önceki sürüme geri alır.
6. **Deployment Ölçeklendirme:**
    - **Pod Sayısını Artırma/Azaltma:**
        - Deployment, dinamik olarak Pod sayısını artırmak veya azaltmak için kullanılabilir. Bu işlem genellikle artan veya azalan kullanıcı talebine göre yapılır.
        - **Komut:** `kubectl scale deployment example-deployment --replicas=5` - Deployment içindeki Pod sayısını 5’e çıkarır.
7. **Deployment İzleme ve Yönetim:**
    - **Durum İzleme:**
        - `kubectl get deployments` komutuyla tüm Deployment’ların durumunu görüntüleyebiliriz.
    - **Detaylı İnceleme:**
        - `kubectl describe deployment example-deployment` komutuyla bir Deployment hakkında ayrıntılı bilgi alabiliriz.
    - **Log İzleme:**
        - `kubectl logs deployment/example-deployment` komutuyla bir Deployment’ın Pod’larından birinin loglarını inceleyebiliriz.
8. **Örnek Kullanım Senaryoları:**
    - **Yeni Sürüm Dağıtımı:**
        - Bir web uygulamasının yeni sürümü dağıtıldığında, Deployment eski sürümü yavaşça kaldırır ve yeni sürümü devreye alır.
    - **Hata Durumunda Geri Alma:**
        - Eğer yeni bir sürümde kritik bir hata oluşursa, Deployment eski sürüme geri döner ve hizmet kesintisini önler.
    - **Otomatik Ölçekleme:**
        - Yoğun trafik altında, Deployment daha fazla Pod başlatarak hizmetin performansını korur ve gerektiğinde Pod sayısını azaltarak kaynak kullanımını optimize eder.

## Sertifikasyon İpucu

Daha önce fark etmiş olabileceğiniz gibi, YAML dosyalarını oluşturmak ve düzenlemek biraz zor olabilir. Özellikle komut satırında (CLI). Sınav sırasında, YAML dosyalarını tarayıcıdan terminale kopyalayıp yapıştırmak zor olabilir. `kubectl run` komutunu kullanmak, bir YAML şablonu oluşturmanıza yardımcı olabilir. Hatta bazen YAML dosyası oluşturmadan sadece `kubectl run` komutunu kullanarak da işinizi görebilirsiniz. Örneğin, sizden belirli bir isim ve image ile bir Pod veya Deployment oluşturmanız istenirse, basitçe `kubectl run` komutunu çalıştırabilirsiniz.

Aşağıdaki komut setini kullanın ve önceki uygulama testlerini tekrar deneyin, ancak bu sefer YAML dosyaları yerine aşağıdaki komutları kullanmayı deneyin. Tüm alıştırmalarda bu komutları olabildiğince çok kullanmaya çalışın.

**NGINX Pod Oluşturma**

```bash
kubectl run nginx --image=nginx
```

**POD Manifest YAML dosyası oluşturun (-o yaml). Dosyayı oluşturmayın (--dry-run)**

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

**Bir deployment oluşturun**

```bash
kubectl create deployment --image=nginx nginx
```

**Deployment YAML dosyası oluşturun (-o yaml). Dosyayı oluşturmayın (--dry-run)**

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

**Deployment YAML dosyası oluşturun (-o yaml). Dosyayı oluşturmayın (--dry-run) ve bir dosyaya kaydedin.**

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

**Dosyada gerekli değişiklikleri yapın (örneğin, daha fazla replica eklemek) ve ardından deployment'ı oluşturun.**

```bash
kubectl create -f nginx-deployment.yaml
```

**VEYA**

Kubernetes sürüm 1.19+ ile, 4 replica ile bir deployment oluşturmak için `--replicas` seçeneğini belirtebiliriz.

```bash
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```

## Kubernetes Services

1. **Service Nedir?**
    - **Tanım:** Service, Kubernetes'te belirli bir grup Pod'a ağ üzerinden erişim sağlamak için kullanılan bir soyutlama katmanıdır. Service, Pod’ların yaşam döngüsünden bağımsız olarak, tutarlı bir erişim noktası sağlar.
    - **Görev:** Service, arkasındaki Pod’ların IP adresleri değişse bile sabit bir IP adresi ve DNS ismi sağlar. Bu sayede, dış dünyadan veya cluster içinden Pod’lara kolayca erişilebilir.
2. **Service Türleri:**

| **Service Türü** | **Açıklama**                                                                                                                                                                                    |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ClusterIP**    | Varsayılan service türüdür. Service sadece cluster içinden erişilebilir. Bir sanal IP adresi (ClusterIP) sağlar ve bu IP üzerinden arkasındaki Pod’lara erişim sağlanır.                        |
| **NodePort**     | Service, her node’un belirli bir portunda (30000-32767 aralığında) erişilebilir hale getirilir. Cluster dışından bu node’un IP adresi ve NodePort kullanılarak erişilebilir.                    |
| **LoadBalancer** | Cloud sağlayıcıları ile entegrasyon sağlayarak dış dünyadan erişilebilen bir yük dengeleyici (Load Balancer) oluşturur. Load Balancer IP adresi üzerinden arkasındaki Pod’lara erişim sağlanır. |
| **ExternalName** | Cluster dışındaki bir DNS ismini çözerek, cluster içinden bu isme erişim sağlar.                                                                                                                |

3. **Service Tanımı (YAML):**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: example-service
spec:
  selector:
    app: webserver
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  type: ClusterIP
```

#### Service Tanımı İçindeki Alanlar ve Olası Değerler

| **Alan**     | **Açıklama**                                                     | **Olası Değerler / Seçenekler**                                                                                                                  |
| ------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `apiVersion` | Kubernetes API sürümünü belirtir.                                | - `v1`                                                                                                                                           |
| `kind`       | Kaynak türünü belirtir.                                          | - `Service`                                                                                                                                      |
| `metadata`   | Service’e ait meta bilgileri içerir, örneğin ad ve etiketler.    | - `name`: Service adı (örn. `example-service`)  <br>- `labels`: Service için etiketler (örn. `app: webserver`)                                   |
| `spec`       | Service’in nasıl çalışacağını tanımlar.                          | - `selector`: Service’in hangi Pod’ları seçeceğini belirlemek için kullanılan etiketler  <br>- `ports`: Service üzerinden kullanılacak portlar   |
| `selector`   | Service’in yöneteceği Pod’ları seçmek için kullanılan etiketler. | - `matchLabels`: Belirli etiketlere sahip Pod’ları seçmek için kullanılır (örn. `app: webserver`)                                                |
| `ports`      | Service üzerinden erişim sağlanacak portları tanımlar.           | - `protocol`: TCP veya UDP  <br>- `port`: Service’in dinleyeceği port (örn. `80`)  <br>- `targetPort`: Pod’larda kullanılacak port (örn. `8080`) |
| `type`       | Service türünü belirtir.                                         | - `ClusterIP`, `NodePort`, `LoadBalancer`, `ExternalName`                                                                                        |

4. **Service’in Çalışma Prensibi:**
    - **Selector ile Pod Seçimi:**
        - Service, `selector` altında tanımlanan etiketlere göre Pod’ları seçer ve bu Pod’lara trafiği yönlendirir. Örneğin, `app=webserver` etiketine sahip tüm Pod’lar Service’in hedefi haline gelir.
    - **Port Yönlendirme:**
        - Service, belirli bir portu dinler ve bu porta gelen trafiği, hedef Pod’ların belirtilen portlarına (targetPort) yönlendirir. Örneğin, Service `80` portunu dinlerken, bu trafiği hedef Pod’ların `8080` portuna yönlendirebilir.
    - **Sabit IP ve DNS Sağlama:**
        - Service, arkasındaki Pod’lar her ne kadar dinamik IP’lere sahip olsa da, sabit bir IP adresi ve DNS adı sağlar. Bu sayede, uygulamanın hangi Pod’lar üzerinde çalıştığına bakılmaksızın, sürekli olarak aynı adrese erişim sağlanır.
5. **Service Türleri ve Kullanım Senaryoları:**
    - **ClusterIP:**
        - **Kullanım:** Sadece cluster içi iletişim için kullanılır. Örneğin, bir web uygulaması ile veritabanı arasında iletişimi sağlamak için.
        - **YAML Örnek:**

        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: webapp-service
        spec:
          selector:
            app: webapp
          ports:
        - protocol: TCP
            port: 80
            targetPort: 8080
          type: ClusterIP
        ```

    - **NodePort:**
        - **Kullanım:** Cluster dışından erişim sağlamak için kullanılır. Örneğin, dış dünyadan bir web uygulamasına erişim sağlamak için.
        - **YAML Örnek:**

        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: webapp-service
        spec:
          selector:
            app: webapp
          ports:
          - protocol: TCP
            port: 80
            targetPort: 8080
            nodePort: 30007
          type: NodePort
        ```

    - **LoadBalancer:**
        - **Kullanım:** Cloud ortamlarında, dış dünyadan gelen trafiği Pod’lara yönlendiren bir yük dengeleyici oluşturur. Örneğin, AWS veya GCP üzerinde çalışan bir Kubernetes cluster’ında.
        - **YAML Örnek:**

        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: webapp-service
        spec:
          selector:
            app: webapp
          ports:
        - protocol: TCP
            port: 80
            targetPort: 8080
          type: LoadBalancer
        ```

    - **ExternalName:**
        - **Kullanım:** Cluster dışındaki bir DNS adına erişim sağlamak için kullanılır. Örneğin, harici bir veritabanı servisine erişim.
        - **YAML Örnek:**

        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: external-db-service
        spec:
          type: ExternalName
          externalName: db.example.com
        ```

1. **Service Yönetimi:**
    - **Service Oluşturma:**
        - `kubectl apply -f service.yaml` komutuyla YAML dosyasındaki Service tanımını uygulayabilirsiniz.
    - **Service İzleme:**
        - `kubectl get services` komutuyla tüm Service’lerin durumu görüntülenir.
    - **Service Detayları:**
        - `kubectl describe service example-service` komutuyla belirli bir Service hakkında detaylı bilgi alabilirsiniz.
    - **Service Silme:**
        - `kubectl delete service example-service` komutuyla Service’i silebilirsiniz.
2. **Service Kullanımının Avantajları:**
    - **Dinamik Pod Yönetimi:**
        - Service, arkasındaki Pod’ların IP adresleri değişse bile sabit bir erişim noktası sağlar. Bu, uygulama güncellemelerini ve ölçeklendirmelerini kolaylaştırır.
    - **Load Balancing:**
        - Service, gelen trafiği otomatik olarak arkasındaki Pod’lara dengeli bir şekilde dağıtır.
    - **Basit Erişim:**
        - Service, kullanıcılar veya diğer uygulamalar için sabit bir IP adresi ve DNS adı sağlar, böylece karmaşık ağ yapılandırmalarına gerek kalmaz.\

## Kubernetes Namespaces

1. **Namespace Nedir?**
    - **Tanım:** Namespace, Kubernetes cluster’ı içinde izole edilmiş sanal ortamlar oluşturan bir mekanizmadır. Farklı projeleri, ekipleri veya çevreleri (development, staging, production) birbirinden ayırmak için kullanılır.
    - **Görev:** Namespace, kaynakları (Pod, Service, Deployment, vs.) gruplamak ve bu grupların birbirinden izole edilmesini sağlamak için kullanılır. Bu, aynı isimdeki kaynakların farklı namespace'lerde var olabilmesini sağlar ve erişim kontrolü ile kaynak sınırlandırması (quota) uygulamalarını kolaylaştırır.
2. **Namespace Kullanım Senaryoları:**
    - **Multi-Tenancy:** Farklı ekiplerin aynı Kubernetes cluster'ını kullanırken kaynaklarının birbirine karışmaması için.
    - **Çevre İzolasyonu:** Development, staging, production gibi farklı ortamlardaki kaynakları birbirinden izole etmek için.
    - **Kaynak Yönetimi:** Belirli bir namespace için kaynak sınırları (quota) ve erişim kontrolü (RBAC) tanımlamak için.
3. **Namespace Tanımı (YAML):**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: example-namespace
```

#### Namespace Tanımı İçindeki Alanlar ve Olası Değerler

| **Alan**     | **Açıklama**                                       | **Olası Değerler / Seçenekler**                    |
| ------------ | -------------------------------------------------- | -------------------------------------------------- |
| `apiVersion` | Kubernetes API sürümünü belirtir.                  | - `v1`                                             |
| `kind`       | Kaynak türünü belirtir.                            | - `Namespace`                                      |
| `metadata`   | Namespace’e ait meta bilgileri içerir, örneğin ad. | - `name`: Namespace adı (örn. `example-namespace`) |

4. **Namespace Türleri:**
    - **Varsayılan Namespace:**
        - **default:** Kubernetes cluster'ında varsayılan olarak bulunan namespace’dir. Namespace belirtilmeyen kaynaklar burada oluşturulur.
    - **Diğer Varsayılan Namespace'ler:**
        - **kube-system:** Kubernetes'in dahili bileşenleri (DNS, dashboard, vs.) bu namespace içinde çalışır.
        - **kube-public:** Cluster genelinde paylaşılan kaynaklar için kullanılan, herkesin erişebileceği bir namespace'dir.
        - **kube-node-lease:** Node'ların durum bilgilerini tutmak için kullanılan, Kubernetes v1.14 ile tanıtılmış bir namespace'dir.
5. **Namespace Oluşturma ve Yönetme:**
    - **Namespace Oluşturma:**
        - `kubectl create namespace <namespace-adı>` komutuyla yeni bir namespace oluşturulabilir.
        - Örnek: `kubectl create namespace dev-environment`
    - **Namespace Listeleme:**
        - `kubectl get namespaces` komutuyla mevcut namespace'leri listeleyebilirsiniz.
    - **Namespace Silme:**
        - `kubectl delete namespace <namespace-adı>` komutuyla bir namespace ve içindeki tüm kaynaklar silinebilir.
    - **Namespace Üzerinde İşlem Yapma:**
        - `kubectl get pods -n <namespace-adı>` komutuyla belirli bir namespace içindeki Pod'lar listelenebilir.
        - Örnek: `kubectl get pods -n dev-environment`
6. **Namespace Kullanımının Avantajları:**
    - **Kaynak İzolasyonu:**
        - Namespace’ler, farklı uygulama veya ekiplerin kaynaklarını birbirinden izole ederek karmaşıklığı azaltır ve karışıklıkları önler.
    - **Aynı İsimde Kaynaklar:**
        - Aynı isimdeki kaynaklar farklı namespace'lerde oluşturulabilir. Örneğin, `frontend` adında bir Pod hem `dev-environment` hem de `prod-environment` namespace'lerinde var olabilir.
    - **Erişim Kontrolü ve Güvenlik:**
        - Role-Based Access Control (RBAC) ve diğer erişim denetim mekanizmaları namespace seviyesinde uygulanabilir. Bu, belirli ekiplerin sadece belirli namespace'lere erişmesini sağlar.
    - **Kaynak Kotaları (Resource Quotas):**
        - Namespace'ler içinde kaynak sınırlamaları tanımlanabilir. Bu, her bir namespace'in belirli miktarda CPU, bellek veya diğer kaynakları kullanabilmesini sağlar.
7. **Namespace Kullanım Senaryoları:**
    - **Çevre İzolasyonu:**
        - Farklı yazılım geliştirme aşamaları (development, staging, production) için ayrı namespace'ler oluşturulabilir. Bu sayede her aşama için ayrı kaynaklar yönetilebilir.
    - **Farklı Ekipler İçin İzolasyon:**
        - Büyük bir Kubernetes cluster'ını birden fazla ekip kullanıyorsa, her ekip için ayrı namespace'ler oluşturulabilir. Her ekip sadece kendi namespace'inde işlem yapabilir.
    - **Kaynak Yönetimi:**
        - Büyük bir cluster'da kaynakların belirli bir düzen içinde kullanılabilmesi için her namespace'e farklı kaynak kotaları uygulanabilir. Bu, kaynakların adil bir şekilde dağıtılmasını sağlar.
8. **Namespace Kullanırken Dikkat Edilmesi Gerekenler:**
    - **Varsayılan Namespace:**
        - Kaynaklar oluşturulurken namespace belirtilmezse, bu kaynaklar varsayılan olarak `default` namespace'inde oluşturulur. Bu nedenle, her zaman doğru namespace'te çalıştığınızdan emin olun.
    - **Erişim Kontrolü:**
        - Namespace'ler arasında geçiş yaparken, hangi namespace'te çalıştığınızı takip edin. Yanlış namespace'te yanlış işlemler yapmaktan kaçının.
    - **Kaynak Kotaları ve Limitler:**
        - Namespace'ler için tanımlanan kaynak kotalarını düzenli olarak gözden geçirin ve gerektiğinde güncelleyin.

## Resource Quotas (Kaynak Kotaları)

1. **Resource Quotas Nedir?**
    - **Tanım:** Resource Quotas, Kubernetes'te belirli bir namespace içinde kullanılabilecek maksimum kaynak miktarını sınırlamak için kullanılan bir mekanizmadır. CPU, bellek, Pod sayısı gibi kaynakların adil ve kontrollü bir şekilde dağıtılmasını sağlar.
    - **Görev:** Belirli bir namespace'in kullanabileceği kaynakları sınırlayarak, cluster içindeki kaynakların tükenmesini veya aşırı kullanımını önler. Bu, özellikle birden fazla ekip veya uygulamanın aynı cluster'ı kullandığı durumlarda çok önemlidir.
2. **Resource Quotas Örnekleri:**

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: example-quota
  namespace: dev-environment
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: "8Gi"
    limits.cpu: "8"
    limits.memory: "16Gi"
```

- **pods:** Bu namespace içinde maksimum 10 Pod çalışabilir.
- **requests.cpu:** Bu namespace içindeki tüm Pod'lar toplamda en fazla 4 CPU birimi talep edebilir.
- **requests.memory:** Bu namespace içindeki tüm Pod'lar toplamda en fazla 8GiB bellek talep edebilir.
- **limits.cpu:** Bu namespace içindeki tüm Pod'lar toplamda en fazla 8 CPU birimi kullanabilir.
- **limits.memory:** Bu namespace içindeki tüm Pod'lar toplamda en fazla 16GiB bellek kullanabilir.

3. **Resource Quotas'ın Avantajları:**
    - **Adil Kaynak Dağıtımı:** Birden fazla ekip aynı cluster'da çalışıyorsa, kaynakların adil bir şekilde dağıtılmasını sağlar.
    - **Kaynak Yönetimi:** Kaynakların aşırı kullanımını engeller ve cluster’ın stabil çalışmasını sağlar.
    - **Maliyet Kontrolü:** Özellikle cloud ortamlarında, fazla kaynak kullanımının önüne geçerek maliyetleri kontrol altında tutar.

## Role-Based Access Control (RBAC)

1. **RBAC Nedir?**
    - **Tanım:** RBAC, Kubernetes'te kullanıcıların (veya servis hesaplarının) hangi kaynaklara nasıl erişebileceğini belirleyen bir güvenlik mekanizmasıdır. Belirli eylemlerin (okuma, yazma, listeleme vb.) belirli kullanıcılar veya gruplar tarafından gerçekleştirilmesine izin verir veya engeller.
    - **Görev:** Kubernetes ortamında kimlerin ne yapabileceğini yönetir. Güvenlik ve erişim denetimlerini sağlar, böylece sadece yetkili kişiler belirli işlemleri gerçekleştirebilir.
2. **RBAC Bileşenleri:**
    - **Role:** Bir namespace içindeki belirli kaynaklar üzerinde yapılabilecek eylemleri tanımlar. Örneğin, belirli bir namespace içinde Pod’ları listeleme ve okuma izni verir.
    - **ClusterRole:** Tüm cluster genelinde geçerli olan rolleri tanımlar. Örneğin, tüm namespace'lerde geçerli olacak şekilde kaynaklara erişim izni verir.
    - **RoleBinding:** Bir Role’ü belirli bir kullanıcıya, grup veya servis hesabına bağlar. Yani, belirli bir Role’e sahip olmayı tanımlar.
    - **ClusterRoleBinding:** Bir ClusterRole’ü belirli bir kullanıcıya, grup veya servis hesabına bağlar.
3. **RBAC Örnekleri:**
**Namespace İçinde Bir Role Tanımlama:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev-environment
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

- **apiGroups:** Hangi API gruplarına erişileceğini belirtir. Boş string (`""`), core API grubunu ifade eder.
- **resources:** Hangi kaynak türlerine (Pod, Service vb.) erişim sağlanacağını belirtir.
- **verbs:** Hangi eylemlere izin verileceğini belirtir (`get`, `list`, `watch`, `create`, `delete` vb.).
**Bu Role'ü Bir Kullanıcıya Bağlama:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: dev-environment
subjects:
- kind: User
  name: jane
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

- **subjects:** Bu role erişim vereceğimiz kullanıcı veya servis hesabını tanımlar.
- **roleRef:** Kullanıcıya veya gruba bağlanacak olan Role’ü belirtir.

4. **RBAC'ın Avantajları:**
    - **Güvenlik:** Kullanıcıların sadece ihtiyaç duydukları kaynaklara erişimini sağlar, gereksiz erişimleri kısıtlar.
    - **Yetki Yönetimi:** Farklı kullanıcı gruplarına farklı erişim seviyeleri tanımlayarak, rolleri ve sorumlulukları net bir şekilde dağıtabilir.
    - **Merkezi Yönetim:** Tüm erişim politikalarını merkezi olarak yönetmenizi sağlar, bu da büyük takımlar için yönetimi kolaylaştırır.

##### Özet

**Resource Quotas (Kotalar):**

- Kaynakların adil kullanımını ve yönetimini sağlamak için kullanılır.
- Kaynakların tükenmesini veya aşırı kullanılmasını engeller.
- Özellikle paylaşımlı ortamlarda maliyet kontrolü ve kaynak yönetimi açısından kritik öneme sahiptir.
**RBAC (Role-Based Access Control):**
- Kubernetes’te kimlerin ne yapabileceğini yönetir.
- Güvenlik ve erişim kontrolü sağlar.
- Rolleri, kullanıcılarla ve gruplarla ilişkilendirerek belirli erişim izinleri verir.
Certification Tips - Imperative Commands with Kubectl

While you would be working mostly the declarative way - using definition files, imperative commands can help in getting one time tasks done quickly, as well as generate a definition template easily. This would help save considerable amount of time during your exams.

Before we begin, familiarize with the two options that can come in handy while working with the below commands:

`--dry-run`: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the `--dry-run=client` option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

`-o yaml`: This will output the resource definition in YAML format on screen.

  

Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.

#### POD

**Create an NGINX Pod**
`kubectl run nginx --image=nginx`

**Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)**
`kubectl run nginx --image=nginx --dry-run=client -o yaml`
#### Deployment

**Create a deployment**
`kubectl create deployment --image=nginx nginx`
**Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)**
`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`
**Generate Deployment with 4 Replicas**
`kubectl create deployment nginx --image=nginx --replicas=4`

You can also scale a deployment using the `kubectl scale` command.
`kubectl scale deployment nginx --replicas=4`

**Another way to do this is to save the YAML definition to a file and modify**
`kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`

You can then update the YAML file with the replicas or any other field before creating the deployment.
#### Service

**Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379**

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`

(This will automatically use the pod's labels as selectors)

Or

`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml` (This will not use the pods labels as selectors, instead it will assume selectors as **app=redis.** [You cannot pass in selectors as an option.](https://github.com/kubernetes/kubernetes/issues/46191) So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)
**Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:**

`kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml`

(This will automatically use the pod's labels as selectors, [but you cannot specify the node port](https://github.com/kubernetes/kubernetes/issues/25478). You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

## `kubectl apply` Komutu

1. **`kubectl apply` Komutu Nedir?**
    
    - **Tanım:** `kubectl apply`, Kubernetes'te belirtilen bir YAML veya JSON dosyasındaki yapılandırmaları cluster’a uygulamak için kullanılan bir komuttur. Bu komut, mevcut kaynakları güncelleyebilir veya yeni kaynaklar oluşturabilir.
    - **Görev:** `kubectl apply`, yapılandırma dosyalarını temel alarak Kubernetes kaynaklarının (Pod, Service, Deployment, vs.) istenen duruma getirilmesini sağlar. Yapılandırma dosyalarında yapılan değişiklikler, bu komutla cluster’a yansıtılır.
2. **`kubectl apply` Komutunun Kullanım Alanları:**
    
    - **Yeni Kaynak Oluşturma:** YAML veya JSON dosyasında tanımlı yeni bir kaynağı cluster’a ekler.
    - **Mevcut Kaynakları Güncelleme:** Var olan kaynakların yapılandırmalarını güncelleyerek istenen duruma getirir.
    - **Deklaratif Yönetim:** Kubernetes kaynaklarını, yapılandırma dosyaları üzerinden deklaratif bir şekilde yönetmeyi sağlar. Bu, özellikle CI/CD süreçlerinde yapılandırma yönetimini kolaylaştırır.
3. **`kubectl apply` Komutunun Temel Kullanımı:**
```bash
kubectl apply -f <dosya-adı.yaml>
```

- **-f**: Uygulanacak yapılandırma dosyasını belirtir.
- **<dosya-adı.yaml>**: YAML veya JSON formatındaki yapılandırma dosyasının adı.

**Örnek:**

```bash
kubectl apply -f deployment.yaml
```

- Bu komut, `deployment.yaml` dosyasındaki yapılandırmayı cluster’a uygular. Eğer kaynaklar zaten mevcutsa, güncellenir; değilse, yeni olarak oluşturulur.

4. **`kubectl apply` Komutunun Özellikleri:**
    
    - **Deklaratif Yaklaşım:** Bu komut, deklaratif yapılandırma yönetimini destekler. Dosyada belirtilen kaynaklar, dosyada tanımlandığı şekilde cluster’a yansıtılır.
    - **Güncellemeler:** Eğer yapılandırma dosyasında bir değişiklik yapılırsa, `kubectl apply` komutunu tekrar çalıştırarak bu değişiklikler cluster’da uygulanabilir.
    - **İdempotentlik:** `kubectl apply` komutu idempotenttir; yani aynı komut birden çok kez çalıştırılsa bile aynı sonuca ulaşılır, gereksiz değişiklikler yapılmaz.
5. **`kubectl apply` Komutunun Alternatif Kullanımları:**
    
    - **Birden Fazla Dosya Uygulama:**
        - Birden fazla YAML veya JSON dosyasını tek bir komutla uygulayabilirsiniz.
        - **Örnek:**
            
		```bash
		kubectl apply -f pod.yaml -f service.yaml
		```
            
    - **Bir Dizindeki Tüm Dosyaları Uygulama:**
        - Bir dizindeki tüm YAML veya JSON dosyalarını tek bir komutla uygulayabilirsiniz.
        - **Örnek:**
	    ```bash
		kubectl apply -f ./k8s-configs/
		```
            
    - **Etiketlerle Kaynakları Filtreleme:**
        - Belirli etiketlere sahip kaynaklara yönelik değişiklikleri uygulamak için kullanılabilir.
        - **Örnek:**
	    ```bash
		kubectl apply -l app=webserver -f deployment.yaml
		```
1. **`kubectl apply` Komutunun Avantajları:**
    
    - **Versiyon Kontrolü:** Yapılandırma dosyaları, versiyon kontrol sistemlerinde saklanarak değişikliklerin takibi yapılabilir.
    - **Kolay Güncelleme:** Mevcut kaynakları güncellemek için sadece yapılandırma dosyasını düzenleyip `kubectl apply` komutunu çalıştırmak yeterlidir.
    - **Otomatik Yönetim:** CI/CD süreçlerinde kolayca otomatikleştirilebilir, böylece yapılandırmaların her zaman güncel kalması sağlanır.
7. **`kubectl apply` ile `kubectl create` Arasındaki Farklar:**
    
    - **`kubectl apply`:** Deklaratif yönetim için kullanılır. Kaynaklar zaten mevcutsa günceller, yoksa yeni kaynak olarak oluşturur. İdempotent bir yaklaşıma sahiptir.
    - **`kubectl create`:** Yeni kaynak oluşturmak için kullanılır. Eğer kaynak zaten varsa hata verir. Güncellemeler için kullanılmaz.
8. **Örnek Senaryolar:**
    
    - **Yeni Bir Deployment Oluşturma:**
        - Bir deployment tanımı olan `nginx-deployment.yaml` dosyasını kullanarak bir deployment oluşturabilirsiniz.
        - **Komut:**
	    ```bash
		kubectl apply -f nginx-deployment.yaml
		```
            
    - **Mevcut Bir Deployment’ı Güncelleme:**
        - Mevcut bir deployment’ın replicas sayısını artırmak için YAML dosyasını güncelleyip tekrar `kubectl apply` komutunu çalıştırabilirsiniz.
        - **Komut:**
	    ```bash
		kubectl apply -f nginx-deployment.yaml
		```
    
	- **ConfigMap veya Secret Güncelleme:**
        - ConfigMap veya Secret dosyalarını güncelleyip `kubectl apply` komutunu kullanarak bu güncellemeleri cluster’a yansıtabilirsiniz.

## Manuel Scheduling (Manuel Planlama)

1. **Manuel Scheduling Nedir?**
    
    - **Tanım:** Kubernetes'te genellikle Pod'ların hangi node üzerinde çalışacağını otomatik bir scheduler belirler. Ancak manuel scheduling, bu süreci kullanıcıya bırakarak Pod'ların belirli node'larda çalışmasını sağlama işlemidir.
    - **Görev:** Manuel scheduling, belirli bir Pod'u belirli bir node'a zorunlu olarak yerleştirmenizi sağlar. Bu, özel gereksinimleri olan iş yükleri için yararlı olabilir, örneğin donanım gereksinimleri veya veri yakınlığı gibi.
2. **Manuel Scheduling Neden Kullanılır?**
    
    - **Özel Donanım Gereksinimleri:** Belirli bir Pod, yalnızca belirli bir node'da bulunan özel donanım kaynaklarını (GPU, SSD vb.) kullanmak zorundaysa.
    - **Veri Yakınlığı:** Pod'un, belirli bir node'da yerel olarak depolanan verilere erişmesi gerektiğinde.
    - **Ağ ve Güvenlik Gereksinimleri:** Belirli bir node'da ağ veya güvenlik politikalarına uygun şekilde çalışması gereken Pod'lar için.
3. **Manuel Scheduling Nasıl Yapılır?**
    
    - Manuel scheduling yapmak için `nodeName` özelliğini kullanabilirsiniz. Bu özellik, bir Pod'un hangi node üzerinde çalışacağını açıkça belirtmenizi sağlar.
4. **Manuel Scheduling Örneği (YAML):**
    
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: nginx
    image: nginx
  nodeName: worker-node-1
```

#### Açıklama:

- **nodeName:** Bu alan, Pod'un hangi node'da çalışacağını belirtir. Bu örnekte, `example-pod` adlı Pod, `worker-node-1` adlı node üzerinde çalışacaktır.

5. **Manuel Scheduling Yaparken Dikkat Edilmesi Gerekenler:**
    
    - **Node’un Durumu:** Pod'un planlandığı node’un çalışır durumda olduğundan ve yeterli kaynağa sahip olduğundan emin olun. Aksi halde Pod, node'a yerleştirilemez ve `Pending` durumda kalır.
    - **Node Etiketlerinin Kullanımı:** Daha esnek bir manuel scheduling için node'ları etiketleyebilir ve bu etiketlere göre Pod'ları planlayabilirsiniz. Bu, node'ların adlarına bağımlı olmadan Pod'ları yönetmenizi sağlar.
    - **Bakım ve Güncellemeler:** Pod'ların belirli node'lara manuel olarak planlanması, bu node'ların bakım ve güncelleme süreçlerini zorlaştırabilir. Bu nedenle manuel scheduling kullanırken dikkatli olmak gerekir.
6. **Node Selector ile Manuel Scheduling:**
    
    - **Node Selector:** Node selector kullanarak, Pod'ları belirli etiketlere sahip node'lara yerleştirebilirsiniz. Bu yöntem, `nodeName` özelliğinden daha esnek bir yapı sağlar.

**Node Selector Örneği:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: nginx
    image: nginx
  nodeSelector:
    disktype: ssd
```


- **nodeSelector:** Bu özellik, Pod'un yalnızca `disktype=ssd` etiketine sahip node'larda çalışmasını sağlar. Bu, belirli donanım özelliklerine sahip node'ları hedeflemek için kullanılabilir.

7. **Taints ve Tolerations ile Manuel Scheduling:**
    - **Taints ve Tolerations:** Node’lara `taint` uygulayarak, belirli Pod'ların bu node'larda çalışmasını engelleyebilir veya belirli Pod'ların bu node'larda çalışmasına izin verebilirsiniz. Toleration ekleyerek, bir Pod'un `taint` edilmiş bir node'da çalışmasına izin verebilirsiniz.

**Taints ve Tolerations Örneği:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "example-key"
    operator: "Exists"
    effect: "NoSchedule"

```

- **tolerations:** Bu özellik, Pod'un belirli bir `taint`e sahip node'da çalışmasına izin verir. Örneğin, `NoSchedule` taint'ine sahip bir node’da bu toleration ile Pod'un çalışmasına izin verilebilir.

8. **Manuel Scheduling Kullanım Senaryoları:**
    - **Özel Gereksinimler:** Bir uygulamanın özel donanım (GPU, NVMe SSD) gereksinimleri varsa, manuel scheduling ile bu gereksinimler karşılanabilir.
    - **Test ve Geliştirme:** Geliştirme ve test ortamlarında belirli node'ları izole etmek veya belirli bir node üzerinde testler yapmak için kullanılabilir.
    - **Veri Yakınlığı:** Veri yoğun uygulamalarda, veri erişimini hızlandırmak için Pod'ları veri kaynaklarına yakın node'lara manuel olarak yerleştirmek.
## Taints ve Tolerations

1. **Taints Nedir?**
    
    - **Tanım:** Taints, Kubernetes’te node’lara eklenen işaretlerdir ve belirli Pod’ların o node’larda çalışmasını engellemek için kullanılır. Bir taint’e sahip node, yalnızca belirli bir toleration’a sahip Pod’ları kabul eder.
    - **Görev:** Taints, belirli bir node’da çalışacak Pod’ları kısıtlayarak iş yüklerinin belirli node’lar üzerinde izole edilmesini sağlar. Örneğin, özel donanım gereksinimleri olan node’lara sadece bu gereksinimleri karşılayan Pod’ların yerleştirilmesini sağlayabilir.
2. **Tolerations Nedir?**
    
    - **Tanım:** Tolerations, Pod’lara eklenen işaretlerdir ve bu Pod’ların belirli taint’lere sahip node’larda çalışmasına izin verir. Toleration, Pod’un bir taint’e sahip node’a yerleşmesine olanak tanır.
    - **Görev:** Tolerations, belirli taint’lere sahip node’larda Pod’ların çalışmasına izin verir. Tolerations olmadan, taint edilmiş bir node’da Pod çalışamaz.
3. **Taints ve Tolerations Nasıl Çalışır?**
    
    - Taints ve tolerations, birlikte çalışarak Kubernetes’te iş yüklerinin belirli node’larda nasıl yerleştirileceğini kontrol eder. Bir taint, node’un üzerinde hangi Pod’ların çalışamayacağını belirtirken, toleration, belirli Pod’ların bu taint’e rağmen node’a yerleşmesine izin verir.
4. **Taints Tanımı:**
	Taints, `kubectl taint nodes` komutu kullanılarak bir node’a eklenir.

	**Taints Ekleme Örneği:**
	
	```bash
	kubectl taint nodes node1 key=value:NoSchedule
	```
#### Açıklama:

- **key=value:** Taint’in anahtar-değer çiftini belirtir.
- **NoSchedule:** Bu etki, toleration’a sahip olmayan Pod’ların bu node’a yerleştirilmesini engeller.

**Taints Efekt Türleri:**

- **NoSchedule:** Taint’lere toleration’a sahip olmayan Pod’ların bu node’da çalışmasını engeller.
- **PreferNoSchedule:** Bu etkiye sahip taint, Kubernetes’in bu node’da Pod çalıştırmaktan kaçınmasına neden olur, ancak zorunlu değildir.
- **NoExecute:** Bu taint, toleration’a sahip olmayan Pod’ların node’dan çıkarılmasına (eviction) neden olur ve yeni Pod’ların bu node’a yerleştirilmesini engeller.

5. **Tolerations Tanımı (YAML):**

Tolerations, Pod tanımı içinde belirtilir ve Pod’un taint’e sahip bir node’da çalışmasına izin verir.

**Tolerations Örneği:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```
#### Açıklama:

- **key:** Toleration’ın uygulanacağı taint’in anahtarını belirtir.
- **operator:** Eşlemenin nasıl yapılacağını belirtir (`Equal` veya `Exists`).
- **value:** Taint’in değeri.
- **effect:** Toleration’ın geçerli olduğu etki türü (`NoSchedule`, `PreferNoSchedule`, `NoExecute`).

6. **Taints ve Tolerations Kullanım Senaryoları:**
    
    - **Özel Donanım Gereksinimleri:** Özel donanım (GPU, SSD) gerektiren Pod’lar için node’ları izole etmek amacıyla kullanılabilir. Örneğin, GPU gerektiren bir node’a taint eklenir, ve bu taint’e toleration’a sahip Pod’lar bu node’a yerleşir.
    - **Test ve Geliştirme:** Belirli node’ları test veya geliştirme ortamlarına ayırmak için kullanılabilir. Diğer Pod’ların bu node’larda çalışmasını engellerken, yalnızca test/geliştirme Pod’larının bu node’lara yerleşmesini sağlar.
    - **Veri Yakınlığı:** Belirli veri kaynaklarına yakın çalışması gereken Pod’ları belirli node’lara zorunlu olarak yerleştirmek için kullanılabilir.
    - **Sorunlu Node’ları İzolasyon:** Bir node’un sorunlu olduğunu biliyorsanız, bu node’u taint ederek yeni Pod’ların bu node’a yerleştirilmesini engelleyebilirsiniz. Böylece mevcut Pod’lar yavaşça başka node’lara taşınabilir.
7. **Taints ve Tolerations ile İlgili Dikkat Edilmesi Gerekenler:**
    
    - **Yanlış Kullanım:** Yanlış taint ve toleration kullanımı, Pod’ların planlanamamasına veya çalışmamasına neden olabilir. Bu nedenle, hangi Pod’un hangi node’da çalışması gerektiği dikkatle planlanmalıdır.
    - **Node İzolasyonu:** Taints, node’ları izole etmek için güçlü bir araçtır, ancak bu node’ların tamamen boş kalmasını da önlemek gerekir. Taint’lerin etkili kullanılmasını sağlamak için doğru tolerations uygulanmalıdır.
    - **Kullanım Senaryoları:** Taints ve tolerations, yalnızca belirli kullanım senaryolarında gerekli olmalıdır. Genel kullanımda, Kubernetes’in scheduler’ına güvenmek genellikle daha etkilidir.

#### A quick note on editing Pods and Deployments
##### Edit a POD

Remember, you CANNOT edit specifications of an existing POD other than the below.

- spec.containers[*].image
    
- spec.initContainers[*].image
    
- spec.activeDeadlineSeconds
    
- spec.tolerations
    

For example you cannot edit the environment variables, service accounts, resource limits (all of which we will discuss later) of a running pod. But if you really want to, you have 2 options:

1. Run the `kubectl edit pod <pod name>` command.  This will open the pod specification in an editor (vi editor). Then edit the required properties. When you try to save it, you will be denied. This is because you are attempting to edit a field on the pod that is not editable.

	A copy of the file with your changes is saved in a temporary location as shown above.
	
	You can then delete the existing pod by running the command:
	
	`kubectl delete pod webapp`
	
	Then create a new pod with your changes using the temporary file
	
	`kubectl create -f /tmp/kubectl-edit-ccvrq.yaml`

2. The second option is to extract the pod definition in YAML format to a file using the command

	`kubectl get pod webapp -o yaml > my-new-pod.yaml`
	
	Then make the changes to the exported file using an editor (vi editor). Save the changes
	
	`vi my-new-pod.yaml`
	
	Then delete the existing pod
	
	`kubectl delete pod webapp`
	
	Then create a new pod with the edited file
	
	`kubectl create -f my-new-pod.yaml`

##### Edit Deployments

With Deployments you can easily edit any field/property of the POD template. Since the pod template is a child of the deployment specification,  with every change the deployment will automatically delete and create a new pod with the new changes. So if you are asked to edit a property of a POD part of a deployment you may do that simply by running the command

`kubectl edit deployment my-deployment`

## Resource Requests ve Limits

1. **Giriş**
   Kubernetes'te **Resource Requests** ve **Limits**, uygulamalarınızın ihtiyaç duyduğu kaynakları (CPU, bellek vb.) tanımlamak ve yönetmek için kullanılır. Bu mekanizma, cluster kaynaklarının etkin bir şekilde kullanılmasını ve uygulamalarınızın stabil çalışmasını sağlar.
2. **Resource Requests ve Limits Nedir?**
   - **Resource Requests (Kaynak Talepleri):**
     - **Tanım:** Bir container'ın düzgün bir şekilde çalışması için minimum olarak ihtiyaç duyduğu kaynak miktarını belirtir.
     - **Görev:** Kubernetes scheduler'ı, Pod'ları node'lara yerleştirirken bu değerleri dikkate alır. Yani, bir Pod'un çalıştırılabilmesi için gereken minimum kaynakları garanti altına alır.
   - **Resource Limits (Kaynak Sınırları):**
     - **Tanım:** Bir container'ın kullanabileceği maksimum kaynak miktarını belirtir.
     - **Görev:** Container'ın aşırı kaynak tüketmesini engeller ve diğer uygulamaların performansını olumsuz etkilemesini önler.
3. **Neden Resource Requests ve Limits Kullanılır?**
   - **Kaynak Yönetimi ve Optimizasyon:**
     - Cluster kaynaklarının adil ve verimli bir şekilde dağıtılmasını sağlar.
     - Aşırı kaynak tüketimini önleyerek diğer uygulamaların performansını korur.
   - **Uygulama Stabilitesi:**
     - Uygulamaların ihtiyaç duydukları minimum kaynakları garanti eder, böylece performans sorunlarını en aza indirir.
   - **Kapasite Planlaması:**
     - Cluster kapasitesinin doğru bir şekilde planlanmasına yardımcı olur.
     - Olası kaynak sıkıntılarını önceden tespit etmeyi ve önlem almayı kolaylaştırır.
4. **CPU ve Bellek (Memory) Kaynaklarının Tanımlanması**
   - **CPU:**
     - **Birimi:** CPU kaynakları millicore olarak tanımlanır.
     - **Örnek:**
       - `500m` = 0.5 CPU (yarım CPU çekirdeği)
       - `1000m` = 1 CPU
       - `2500m` = 2.5 CPU
   - **Bellek (Memory):**
     - **Birimi:** Bellek kaynakları byte cinsinden tanımlanır, ancak daha okunabilir olması için `Ki`, `Mi`, `Gi` gibi kısaltmalar kullanılır.
     - **Örnek:**
       - `512Mi` = 512 Mebibyte
       - `1Gi` = 1 Gibibyte
       - `2Gi` = 2 Gibibyte

**Birimler Tablosu:**

|**Birimi**|**Anlamı**|**Byte Cinsinden**|
|---|---|---|
|`Ki`|Kibibyte|1024 Byte|
|`Mi`|Mebibyte|1024 KiB = 1,048,576 Byte|
|`Gi`|Gibibyte|1024 MiB = 1,073,741,824 Byte|
|`Ti`|Tebibyte|1024 GiB|
|`Pi`|Pebibyte|1024 TiB|
|`Ei`|Exbibyte|1024 PiB|

5. **Resource Requests ve Limits Nasıl Tanımlanır?**
   Resource requests ve limits, Pod tanımı içindeki her bir container için ayrı ayrı belirtilir.
   **Örnek YAML Tanımı:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-demo-pod
spec:
  containers:
    - name: resource-demo-container
      image: nginx
      resources:
        requests:
          cpu: "500m"
          memory: "256Mi"
        limits:
          cpu: "1000m"
          memory: "512Mi"
```
**Açıklama:**
- **requests:**
  - `cpu`: Container'ın minimum 0.5 CPU çekirdeğine ihtiyaç duyduğunu belirtir.
  - `memory`: Container'ın minimum 256Mi belleğe ihtiyaç duyduğunu belirtir.
- **limits:**
  - `cpu`: Container'ın maksimum 1 CPU çekirdeği kullanabileceğini belirtir.
  - `memory`: Container'ın maksimum 512Mi bellek kullanabileceğini belirtir.
6. **Kubernetes'in Resource Requests ve Limits Kullanımı**
   - **Scheduling (Planlama):**
     - Kubernetes scheduler'ı, Pod'ları node'lara yerleştirirken **requests** değerlerini dikkate alır.

     - Scheduler, node'un mevcut kaynaklarını kontrol eder ve Pod'un taleplerini karşılayabilecek bir node seçer.
   - **Enforcement (Zorunluluk):**
     - **CPU Limits:**
       - Container, belirtilen CPU limitini aşmaya çalışırsa Kubernetes, cgroups aracılığıyla CPU kullanımını sınırlayarak aşırı tüketimi engeller.
       - CPU kaynakları burstable olduğu için, eğer node'da boşta CPU varsa container geçici olarak limitinin üzerinde CPU kullanabilir, ancak diğer uygulamalar kaynak talep ettiğinde Kubernetes CPU kullanımını sınırlar.
     - **Memory Limits:**
       - Container, belirtilen bellek limitini aşmaya çalışırsa Kubernetes, Out Of Memory (OOM) hatası vererek container'ı sonlandırır.
       - Bellek kaynakları oversubscribe edilemez; yani, limitin üzerinde bellek kullanımı kabul edilmez.
7. **Quality of Service (QoS) Sınıfları**
   Kubernetes, resource requests ve limits değerlerine göre Pod'lara farklı **QoS sınıfları** atar:

  |**QoS Sınıfı**|**Koşullar**|**Özellikler**|
|---|---|---|
|**Guaranteed**|- Tüm container'lar için hem requests hem de limits değerleri tanımlanmış ve eşit olmalı.|- En yüksek öncelik.  <br>- Kaynak sıkıntısı durumunda son olarak bu Pod'lar sonlandırılır.|
|**Burstable**|- En az bir container için requests ve limits değerleri tanımlanmış ancak eşit değil.|- Orta öncelik.  <br>- Kaynaklar müsaitse limitlere kadar kullanabilirler.|
|**BestEffort**|- Hiçbir container için requests veya limits değerleri tanımlanmamış.|- En düşük öncelik.  <br>- Kaynak sıkıntısı durumunda ilk olarak bu Pod'lar sonlandırılır.

   **Örnek QoS Sınıfı Tanımları:**
   **Guaranteed Pod Örneği:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: guaranteed-pod
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        cpu: "500m"
        memory: "256Mi"
      limits:
        cpu: "500m"
        memory: "256Mi"
```
   **Burstable Pod Örneği:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: burstable-pod
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        cpu: "500m"
        memory: "256Mi"
      limits:
        cpu: "1000m"
        memory: "512Mi"
```
   **BestEffort Pod Örneği:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: besteffort-pod
spec:
  containers:
  - name: nginx
    image: nginx
```
8. **Resource Quota ile Entegrasyon**
   Namespace seviyesinde **Resource Quota** tanımlayarak, o namespace içindeki toplam kaynak kullanımını sınırlayabilirsiniz. Bu, özellikle multi-tenant ortamlarda kaynakların adil dağıtılmasını sağlar.
   **Örnek Resource Quota Tanımı:**
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-namespace-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: "8Gi"
    limits.cpu: "8"
    limits.memory: "16Gi"
```
**Açıklama:**
- Bu quota, `dev` namespace'i içinde maksimum 10 Pod, 4 CPU ve 8Gi bellek talebi, 8 CPU ve 16Gi bellek limiti olacağını belirtir.
9. **En İyi Uygulamalar**
   - **Doğru Değerleri Belirleyin:**
     - Uygulamanızın gerçek kaynak ihtiyaçlarını izleyin ve buna göre requests ve limits değerlerini belirleyin.
     - Gereğinden düşük veya yüksek değerler performans sorunlarına veya kaynak israfına yol açabilir.
   - **Requests ve Limits Arasında Mantıklı Bir Oran Oluşturun:**
     - Limits değerleri, requests değerlerinden çok fazla farklı olmamalı. Özellikle bellek için bu farkın çok büyük olmamasına dikkat edin.
   - **QoS Sınıflarını Kullanın:**
     - Kritik uygulamalar için **Guaranteed** sınıfını tercih edin.
     - Daha az kritik veya esnek uygulamalar için **Burstable** veya **BestEffort** sınıflarını kullanabilirsiniz.
   - **Monitoring ve Logging:**
     - Uygulamalarınızın kaynak kullanımını sürekli izleyin. Prometheus, Grafana gibi araçlar kullanarak metrikleri toplayın ve analiz edin.
     - OOMKilled gibi olayları loglardan takip ederek kaynak limitlerini gerektiğinde ayarlayın.
   - **Resource Quota Tanımlayın:**
     - Özellikle paylaşımlı ortamlarda, her namespace için uygun resource quota'lar tanımlayarak kaynakların adil ve kontrollü bir şekilde kullanılmasını sağlayın.
10. **Sorun Giderme**
    - **Pod Pending Durumunda Kalıyorsa:**
      - Nedeni: Scheduler, belirtilen requests değerlerini karşılayacak yeterli kaynağa sahip bir node bulamıyor olabilir.
      - Çözüm: Requests değerlerini düşürün veya cluster kapasitesini artırın.
    - **Container OOMKilled Oluyorsa:**
      - Nedeni: Container, belirtilen bellek limitini aşıyor.
      - Çözüm: Bellek limitini artırın veya uygulamanın bellek kullanımını optimize edin.
    - **Yüksek CPU Kullanımı:**
      - Nedeni: Uygulama, belirtilen CPU limitini aşıyor ve performans sorunları yaşıyor olabilir.
      - Çözüm: CPU limitini artırın veya uygulamanın CPU kullanımını optimize edin.
## DaemonSets

1. **DaemonSet Nedir?**
    - **Tanım:** DaemonSet, Kubernetes'te her bir node üzerinde bir kopya Pod çalıştırmak için kullanılan bir mekanizmadır. DaemonSet tarafından yönetilen Pod'lar, cluster’daki her bir node üzerinde otomatik olarak çalıştırılır.
    - **Görev:** DaemonSet, node seviyesinde hizmet vermesi gereken işleri (log toplama, monitoring, network yönetimi vb.) her node üzerinde dağıtır. Böylece, bu hizmetlerin tüm node'larda kesintisiz olarak çalışması sağlanır.
2. **DaemonSet’in Kullanım Alanları:**
    - **Node Seviyesi İzleme ve Log Toplama:** Prometheus Node Exporter, Fluentd gibi araçları her node’da çalıştırarak sistem metriklerini toplama ve log yönetimini sağlama.
    - **Network Yönetimi:** CNI (Container Network Interface) eklentileri gibi her node üzerinde çalışması gereken ağ bileşenlerinin dağıtılması.
    - **Node Bakım ve Yönetimi:** Her node’da bir sistem hizmeti çalıştırarak node’ların durumunu izleme veya yönetme işlemleri.
3. **DaemonSet Nasıl Çalışır?**
    - DaemonSet, cluster'daki her node üzerinde bir Pod çalıştırır. Yeni bir node eklendiğinde, DaemonSet bu node üzerinde de otomatik olarak Pod başlatır.
    - Eğer bir node cluster'dan çıkarılırsa, bu node üzerindeki DaemonSet Pod'ları da otomatik olarak silinir.
    - DaemonSet, sadece belirli node'lar üzerinde Pod çalıştıracak şekilde yapılandırılabilir (örneğin, etiketlere göre).
4. **DaemonSet Tanımı (YAML):**

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
  labels:
    app: example
spec:
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: example-container
        image: nginx
```
#### Açıklama:
- **metadata.name:** DaemonSet'in adı.
- **selector.matchLabels:** Hangi Pod'ların bu DaemonSet tarafından yönetileceğini belirtir.
- **template:** DaemonSet'in her node üzerinde başlatacağı Pod'un tanımı.
    - **containers:** Her Pod içinde çalıştırılacak container’ların listesi. Bu örnekte, `nginx` container’ı çalıştırılmaktadır.
5. **DaemonSet’in Avantajları:**
    - **Node Bazlı İşlemler:** Her bir node üzerinde çalışması gereken işleri otomatikleştirir ve merkezi bir şekilde yönetilmesini sağlar.
    - **Otomatik Ölçeklenebilirlik:** Yeni node’lar cluster’a eklendiğinde, DaemonSet bu node'larda otomatik olarak Pod başlatır.
    - **Kolay Yönetim:** DaemonSet, her node üzerinde aynı işi çalıştırarak yönetimi basitleştirir. Tek bir DaemonSet tanımıyla tüm node'ları yönetebilirsiniz.
6. **DaemonSet ile Çalışırken Dikkat Edilmesi Gerekenler:**
    - **Node Kaynakları:** DaemonSet ile her node’da çalışacak Pod’lar, node’un kaynaklarını kullanır. DaemonSet Pod'ları, diğer iş yükleriyle rekabet edeceği için kaynak planlaması önemlidir.
    - **Node Seçimi:** Eğer belirli node’lar üzerinde DaemonSet Pod'larını çalıştırmak istemiyorsanız, nodeSelector veya tolerations kullanarak bu node’ları hariç tutabilirsiniz.
    - **Update Strategy (Güncelleme Stratejisi):** DaemonSet’in Pod’larını güncellerken dikkatli olunmalıdır. `RollingUpdate` stratejisi ile Pod’lar sırayla güncellenebilir, bu da hizmet kesintisini önler.
7. **DaemonSet Güncelleme Stratejileri:**
    - **RollingUpdate:** DaemonSet Pod’larını sırayla günceller. Bu, node'lar üzerindeki iş yükünün kesintisiz devam etmesini sağlar.
    - **OnDelete:** DaemonSet, Pod'ları manuel olarak silinene kadar güncellemeyecek. Güncellemenin elle yönetilmesi gerektiğinde kullanılır.
    **Güncelleme Stratejisi Örneği:**
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
spec:
  updateStrategy:
    type: RollingUpdate
```
8. **Örnek Senaryolar:**
    - **Log Toplama:** Tüm node’larda log toplama agent'larını çalıştırmak için bir DaemonSet kullanabilirsiniz. Örneğin, Fluentd veya Logstash gibi araçlarla tüm node’larda log toplayabilirsiniz.
    - **Node İzleme:** Prometheus Node Exporter gibi bir araçla her node’daki sistem metriklerini toplamak için DaemonSet kullanabilirsiniz.
    - **Ağ Eklentisi Yönetimi:** Her node üzerinde bir ağ eklentisi (örn. Calico, Weave) çalıştırarak, ağ yönetimini ve güvenliğini sağlayabilirsiniz.
9. **DaemonSet ile Hedeflenmiş Node Seçimi:**
    - **NodeSelector:** DaemonSet Pod'larını sadece belirli etiketlere sahip node'larda çalıştırmak için kullanılır.
    - **Taints ve Tolerations:** Belirli taint’lere sahip node’larda DaemonSet Pod'larını çalıştırmak için kullanılır.
    
    **NodeSelector Örneği:**
    
	```yaml
	apiVersion: apps/v1
	kind: DaemonSet
	metadata:
	  name: example-daemonset
	spec:
	  selector:
	    matchLabels:
	      app: example
	  template:
	    metadata:
	      labels:
	        app: example
	    spec:
	      nodeSelector:
	        disktype: ssd
	      containers:
	      - name: example-container
	        image: nginx
	```
    
	- **Açıklama:** Bu örnekte, DaemonSet yalnızca `disktype=ssd` etiketine sahip node'larda çalışacak Pod'lar oluşturacaktır.
10. **Sorun Giderme:**
    - **Pod Çalışmıyorsa:**
        - **Nedeni:** Pod, kaynak eksikliği, node seçimi veya toleration eksikliği nedeniyle çalışmıyor olabilir.
        - **Çözüm:** Pod loglarını ve event’leri inceleyin. Node üzerinde yeterli kaynak olduğundan ve nodeSelector/tolerations kurallarının doğru tanımlandığından emin olun.
    - **DaemonSet Güncellemesi Başarısız Olursa:**
        - **Nedeni:** Güncelleme sırasında Pod'lar başarılı bir şekilde yeniden başlatılamamış olabilir.
        - **Çözüm:** Güncelleme stratejinizi gözden geçirin ve gerekirse manuel olarak müdahale edin.
## Static Pods

1. **Static Pods Nedir?**
    
    - **Tanım:** Static Pods, Kubernetes'te kubelet tarafından doğrudan bir node üzerinde yönetilen Pod'lardır. Bu Pod'lar, Kubernetes API server tarafından yönetilmez ve genellikle master node üzerinde veya özel durumlar için kullanılır.
    - **Görev:** Static Pods, özellikle Kubernetes'in yönetim bileşenlerini (API server, etcd, scheduler gibi) çalıştırmak veya node üzerinde manuel olarak yönetilen uygulamaları çalıştırmak için kullanılır. Static Pod'lar, kubelet tarafından sürekli izlenir ve eğer silinirse, kubelet tarafından yeniden başlatılır.
2. **Static Pods ile Normal Pods Arasındaki Farklar:**
    
    - **Yönetim:** Normal Pod'lar Kubernetes API server üzerinden yönetilirken, Static Pod'lar doğrudan kubelet tarafından yönetilir.
    - **Tanımlama:** Normal Pod'lar bir Deployment, ReplicaSet veya başka bir Kubernetes nesnesi aracılığıyla tanımlanırken, Static Pod'lar node üzerinde bir dosya olarak tanımlanır.
    - **API Server’dan Bağımsızlık:** Static Pod'lar, Kubernetes API server çalışmasa bile kubelet tarafından yönetilmeye devam eder.
3. **Static Pod Tanımları:**
    
    - Static Pod'lar, bir YAML dosyası olarak node'un dosya sisteminde belirli bir dizine yerleştirilir. Bu dizin, genellikle kubelet'in yapılandırma dosyasında belirtilir.
    - Varsayılan olarak, kubelet `/etc/kubernetes/manifests` dizininde bulunan YAML dosyalarını tarar ve bu dosyalara göre Static Pod'ları başlatır.
4. **Static Pod Tanımı (YAML):**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-pod-example
  labels:
    app: static-pod
spec:
  containers:
  - name: nginx
    image: nginx
```

#### Açıklama:

- **metadata.name:** Static Pod'un adı.
- **spec.containers:** Bu Static Pod içinde çalışacak container’ın tanımı. Bu örnekte, `nginx` container'ı çalıştırılmaktadır.

5. **Static Pod'lar Nasıl Çalışır?**
    
    - **Manuel Tanımlama:** Bir Static Pod tanımlamak için, Pod tanımı bir YAML dosyası olarak belirlenen dizine yerleştirilir. Kubelet, bu dizini izler ve dosya eklendiğinde, Static Pod’u başlatır.
    - **Yeniden Başlatma:** Static Pod silindiğinde veya başarısız olduğunda, kubelet bu Pod'u otomatik olarak yeniden başlatır.
    - **API Server Üzerinde Görünürlük:** Static Pod'lar, Kubernetes API server üzerinden listelenebilir, ancak API server bu Pod'ları yönetmez.
6. **Static Pods Kullanım Senaryoları:**
    
    - **Kubernetes Bileşenleri:** Kubernetes'in kendi bileşenlerini (API server, scheduler, controller manager) çalıştırmak için Static Pod'lar kullanılır. Bu bileşenler genellikle master node üzerinde çalışır.
    - **Kritik Sistem İş Yükleri:** API server'dan bağımsız olarak çalışması gereken kritik iş yükleri için kullanılabilir. Örneğin, API server'ın arızalanması durumunda bile çalışmaya devam etmesi gereken uygulamalar.
7. **Static Pods ile Çalışırken Dikkat Edilmesi Gerekenler:**
    
    - **Kubelet Yapılandırması:** Static Pod'ların tanımlandığı dizin, kubelet yapılandırma dosyasında doğru bir şekilde belirtilmelidir.
    - **Manuel Müdahale Gerekliliği:** Static Pod'lar Kubernetes tarafından otomatik olarak yönetilmediğinden, bu Pod'ları yönetmek için manuel müdahale gerekebilir. Bu durum, Pod'ları güncellerken veya silerken dikkat edilmesi gerektiği anlamına gelir.
    - **Log ve İzleme:** Static Pod'ların logları ve durumları Kubernetes API server üzerinden izlenebilir, ancak bu Pod'ların yönetimi için kubelet yapılandırmalarına dikkat edilmelidir.
8. **Static Pods ile İlgili Önemli Noktalar:**
    
    - **Otomatik Yeniden Başlatma:** Static Pod'lar kubelet tarafından sürekli izlenir ve herhangi bir sebepten ötürü kapanırlarsa otomatik olarak yeniden başlatılır.
    - **Node'lar Üzerinde Dağıtım:** Static Pod'lar, her bir node üzerinde manuel olarak tanımlandığı için, tüm node'larda çalışacak Static Pod'ları dağıtmak zaman alıcı olabilir.
    - **Master Node’larda Kullanım:** Kubernetes’in kontrol bileşenleri (API server, scheduler) genellikle master node üzerinde Static Pod olarak çalıştırılır.
9. **Static Pods’un API Server Üzerinde Görüntülenmesi:**
    
    - Kubernetes API server, Static Pod'ları otomatik olarak listeleyebilir ve izleyebilir. `kubectl get pods` komutuyla Static Pod'lar, diğer Pod'larla birlikte listelenir, ancak yönetimleri API server'dan yapılmaz.
10. **Örnek Senaryo:**
    
    - **Kubernetes API Server’ı Static Pod Olarak Çalıştırma:**
        - API server, kubelet tarafından Static Pod olarak çalıştırılır. Bu sayede, Kubernetes API server'ı kendini yönetebilir ve herhangi bir aksama durumunda kubelet tarafından otomatik olarak yeniden başlatılabilir.
        - **Komut:**
	```bash
	# Kubelet tarafından izlenen dizine static pod tanımını yerleştir
	sudo mv /path/to/static-pod.yaml /etc/kubernetes/manifests/
	```

11. **Static Pod Sorun Giderme:**
    
    - **Static Pod Başlamıyorsa:**
        - **Nedeni:** Kubelet yapılandırma dosyasında yanlış dizin belirtilmiş olabilir veya YAML dosyasında bir hata olabilir.
        - **Çözüm:** Kubelet yapılandırmasını kontrol edin ve dosyanın doğru dizinde olduğundan emin olun. YAML dosyasını hatalara karşı kontrol edin.
    - **Static Pod Beklenmedik Şekilde Kapanıyorsa:**
        - **Nedeni:** Pod, kaynak yetersizliği veya yanlış yapılandırma nedeniyle kapanıyor olabilir.
        - **Çözüm:** Pod loglarını kontrol edin ve kubelet’in yeniden başlatma davranışını izleyin. Gerekirse kaynakları artırın veya Pod yapılandırmasını düzeltin.

### Multiple Schedulers (Birden Fazla Scheduler) Notları

1. **Multiple Schedulers Nedir?**
    - **Tanım:** Kubernetes, varsayılan olarak tek bir scheduler (zamanlayıcı) ile gelir. Ancak, cluster’ınızda aynı anda birden fazla scheduler çalıştırmak mümkündür. Bu, farklı iş yüklerini veya özel gereksinimleri olan Pod'ları farklı schedulers ile yönetmenizi sağlar.
    - **Görev:** Varsayılan scheduler dışında kendi özel scheduler'ınızı oluşturup çalıştırarak, belirli iş yüklerinin daha özelleşmiş kurallar doğrultusunda planlanmasını sağlayabilirsiniz. Örneğin, belirli bir iş yükü için farklı kaynak kullanımı veya belirli node’lara öncelik tanıyan bir scheduler tanımlayabilirsiniz.
2. **Neden Multiple Schedulers Kullanılır?**
    - **Özelleştirilmiş Planlama:** Farklı Pod'lar için farklı planlama kriterleri uygulamak istiyorsanız, birden fazla scheduler kullanabilirsiniz. Örneğin, belirli bir iş yükü için özel bir scheduling algoritması veya öncelik tanımlayabilirsiniz.
    - **Kaynak Optimizasyonu:** Farklı scheduler'lar, kaynakların daha iyi optimize edilmesine yardımcı olabilir. Örneğin, CPU ve bellek yoğun iş yüklerini ayrı bir scheduler ile yöneterek daha dengeli bir kaynak dağılımı sağlayabilirsiniz.
    - **Özel Donanım Gereksinimleri:** Özel donanımlara (GPU, SSD vb.) sahip node’lara yerleştirilmesi gereken Pod'lar için özelleşmiş scheduler'lar kullanılabilir.
3. **Multiple Schedulers Nasıl Çalışır?**
    - Kubernetes'te birden fazla scheduler çalıştırmak için, ek scheduler’ların kendi YAML yapılandırma dosyalarıyla tanımlanması gerekir. Scheduler'lar, kube-scheduler gibi bir deployment olarak çalıştırılabilir.
    - Pod'lar, hangi scheduler tarafından yönetileceklerini `schedulerName` alanı ile belirtirler. Bu alanı belirterek, Pod'un belirli bir scheduler ile yerleştirilmesini sağlayabilirsiniz.
4. **Varsayılan Scheduler ile Özel Scheduler Arasındaki Farklar:**
    - **Varsayılan Scheduler:** Tüm Pod'lar varsayılan olarak `default-scheduler` tarafından planlanır.
    - **Özel Scheduler:** Eğer Pod'un `schedulerName` alanı belirtilirse, Pod yalnızca belirtilen scheduler tarafından yönetilir.
5. **Multiple Schedulers Yapılandırması:**
    **Adım 1: Özel Scheduler'ı Tanımlama**
    - Öncelikle özel bir scheduler oluşturmak için bir YAML dosyası ile kendi scheduler’ınızı tanımlamanız gerekir.
    **Özel Scheduler Deployment Tanımı (YAML):**

	```yaml
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: custom-scheduler
	spec:
	  replicas: 1
	  selector:
	    matchLabels:
	      component: custom-scheduler
	  template:
	    metadata:
	      labels:
	        component: custom-scheduler
	    spec:
	      containers:
	      - name: custom-scheduler
	        image: k8s.gcr.io/kube-scheduler:v1.21.0
	        command:
	        - kube-scheduler
	        - --config=/etc/kubernetes/config/custom-scheduler-config.yaml
	        volumeMounts:
	        - name: config-volume
	          mountPath: /etc/kubernetes/config
	      volumes:
	      - name: config-volume
	        configMap:
	          name: custom-scheduler-config
	```
    
    **Açıklama:**
    - **name:** `custom-scheduler`, bu deployment’da özel scheduler'ı çalıştıracak.
    - **image:** Kubernetes’in varsayılan scheduler image’i kullanılıyor, ancak komut satırında özel bir yapılandırma dosyasıyla çalıştırılıyor.
    - **configMap:** Scheduler için gerekli yapılandırmaların bulunduğu ConfigMap tanımlanıyor.
    
    **Adım 2: Özel Scheduler Yapılandırma Dosyası**
    - Özel scheduler'ın nasıl çalışacağını belirleyen bir yapılandırma dosyası tanımlayın.
    **Özel Scheduler ConfigMap Tanımı (YAML):**
	    
	```yaml
	apiVersion: v1
	kind: ConfigMap
	metadata:
	  name: custom-scheduler-config
	  namespace: kube-system
	data:
	  custom-scheduler-config.yaml: |
	    apiVersion: kubescheduler.config.k8s.io/v1
	    kind: KubeSchedulerConfiguration
	    clientConnection:
	      kubeconfig: "/etc/kubernetes/scheduler.conf"
	    leaderElection:
	      leaderElect: true
	    schedulerName: custom-scheduler
	```

    **Açıklama:**
    
    - **schedulerName:** Özel scheduler'ın ismi, `custom-scheduler` olarak belirlenmiştir.
6. **Pod'larda Özel Scheduler Kullanımı:**
    
    **Özel Scheduler ile Bir Pod Tanımı (YAML):**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: nginx
	    image: nginx
	  schedulerName: custom-scheduler
	```
    
    **Açıklama:**
    - **schedulerName:** `custom-scheduler` olarak tanımlanan bu alan, Pod'un özel scheduler tarafından planlanacağını belirtir.
7. **Multiple Schedulers Kullanım Senaryoları:**
    - **Özel Donanım:** GPU, SSD gibi özel donanımlara ihtiyaç duyan iş yükleri için farklı bir scheduler tanımlayarak, sadece bu iş yüklerini ilgili node’lara yerleştirebilirsiniz.
    - **Farklı Algoritmalar:** Varsayılan scheduler’ın ötesinde, belirli bir iş yükü için özel scheduling algoritmalarına ihtiyaç duyuyorsanız, birden fazla scheduler ile iş yüklerini yönetebilirsiniz.
    - **Düşük Gecikme:** Gerçek zamanlı veya düşük gecikme gerektiren iş yüklerini yönetmek için özelleşmiş bir scheduler kullanarak iş yüklerini daha optimize bir şekilde planlayabilirsiniz.
8. **Multiple Schedulers ile Dikkat Edilmesi Gerekenler:**
    - **Koordinasyon:** Farklı scheduler'ların aynı node üzerinde çalışması durumunda kaynak yarışmaları olabilir. Scheduler'lar arasında doğru bir koordinasyon sağlamak önemlidir.
    - **Karmaşıklık:** Birden fazla scheduler kullanmak cluster yönetimini daha karmaşık hale getirebilir. Özellikle scheduler'lar arasında doğru bir denge sağlanmalı ve iş yükleri doğru planlanmalıdır.
    - **Performans İzleme:** Her scheduler’ın performansını ve iş yükü yönetimini düzenli olarak izlemek, potansiyel sorunları önlemek için kritik öneme sahiptir.
9. **Multiple Schedulers Sorun Giderme:**
    - **Pod Planlanmıyorsa:**
        - **Nedeni:** Pod, belirtilen scheduler tarafından planlanamayabilir.
        - **Çözüm:** Pod’un `schedulerName` alanını kontrol edin ve özel scheduler’ın doğru çalıştığından emin olun. Ayrıca, logları inceleyin ve kube-scheduler pod’unda hataları arayın.
    - **Scheduler Çakışması:**
        - **Nedeni:** Birden fazla scheduler aynı node'lar üzerinde çalışıyorsa, kaynak yarışmaları olabilir.
        - **Çözüm:** Scheduler'lar arasında daha iyi kaynak yönetimi sağlayacak kurallar ekleyin ve gerektiğinde toleration veya nodeSelector kullanarak scheduler'ları belirli node’larla sınırlandırın.
### Logging ve Monitoring Notları
1. **Logging ve Monitoring Nedir?**
    - **Logging (Kayıt Tutma):** Kubernetes cluster’ında çalışan uygulamaların, sistem bileşenlerinin ve Pod'ların aktivitelerini kaydederek bu verileri izlemeye, analiz etmeye olanak tanır. Bu kayıtlar, hata ayıklama, performans sorunlarını çözme ve sistemin sağlığını izlemek için kritik öneme sahiptir.
    - **Monitoring (İzleme):** Kubernetes cluster’ındaki sistem kaynaklarının, uygulamaların ve bileşenlerin metriklerini toplar, izler ve analiz eder. Monitoring, sistemin performansını izlemek ve potansiyel sorunları erken tespit etmek için gereklidir.
2. **Neden Logging ve Monitoring Kullanılır?**
    - **Sorun Tespiti ve Çözümü:** Uygulamalarda veya sistemde oluşan sorunları tespit etmek ve çözmek için detaylı log verilerine ihtiyaç duyulur. Monitoring ise, performans sorunlarını tespit ederek çözüm sağlamaya yardımcı olur.
    - **Sistem Sağlığını İzleme:** Sistem ve uygulama metrikleri toplanarak, sistemin genel sağlığı sürekli olarak izlenir. Bu, uygulamaların stabil çalışmasını ve kaynakların verimli kullanılmasını sağlar.
    - **Ölçeklenebilirlik:** Büyük bir Kubernetes cluster’ında, merkezi logging ve monitoring araçları kullanarak iş yüklerinin ve bileşenlerin durumunu hızlıca analiz etmek mümkündür.
3. **Logging ve Monitoring Araçları:**
    - **Logging Araçları:**
        - **Fluentd:** Log verilerini toplar, filtreler ve çeşitli hedeflere (Elasticsearch, Splunk, GCP, AWS vb.) gönderir.
        - **Logstash:** Logları toplar, işleyip analiz edilecek yerlere gönderir (Elasticsearch ile yaygın olarak kullanılır).
        - **Elasticsearch:** Logları saklamak ve hızlı arama yapabilmek için kullanılan dağıtık bir arama motorudur.
        - **Kibana:** Elasticsearch ile birlikte çalışarak logları görselleştirmek için kullanılan bir arayüzdür.
    - **Monitoring Araçları:**
        - **Prometheus:** Kubernetes cluster'ındaki metrikleri toplamak için kullanılan güçlü bir monitoring aracıdır. Otomatik keşif (service discovery) özellikleri sayesinde Kubernetes ile uyumlu çalışır.
        - **Grafana:** Prometheus gibi metrik toplama sistemlerinden verileri çekerek grafikler ve paneller oluşturur.
        - **Alertmanager:** Prometheus ile entegre çalışarak kritik durumlar için alarmlar ve bildirimler ayarlar.
        - **cAdvisor:** Her node’daki container’ların kaynak kullanım metriklerini toplar.
4. **Logging Yapısı:**
    Kubernetes'te log verileri iki ana düzeyde toplanır:
    - **Node Seviyesi Loglama:**
        - Her bir node, kubelet tarafından üretilen logları tutar. Node üzerindeki container’lar tarafından üretilen loglar, bu loglar ile birleşir. Bu loglar genellikle `/var/log/containers/` dizininde saklanır.
    - **Pod ve Container Seviyesi Loglama:**
        - Kubernetes'te her bir container’ın standart çıktılarına (`stdout` ve `stderr`) erişim mümkündür. `kubectl logs <pod-name>` komutuyla bu loglara erişilebilir.
    **Örnek: Fluentd ile Merkezi Log Toplama**
    1. Fluentd, her node üzerinde bir DaemonSet olarak çalıştırılır.
    2. Fluentd, node üzerindeki container loglarını toplar ve belirli bir hedefe (örneğin, Elasticsearch) gönderir.
    3. Elasticsearch, topladığı logları saklar ve analiz edilecek hale getirir.
    4. Kibana gibi bir arayüz kullanılarak bu loglar analiz edilebilir.
5. **Monitoring Yapısı:**
    Kubernetes'te monitoring, özellikle metrik toplama, analiz ve alarm oluşturma üzerine odaklanır.
    - **Prometheus ile Monitoring:**
        - Prometheus, Kubernetes içindeki Pod’lar, node'lar ve cluster bileşenlerinden metrikleri toplamak için bir ServiceDiscovery mekanizması kullanır. Bu mekanizma sayesinde, yeni eklenen bileşenler otomatik olarak izlenebilir.
    - **Metrik Toplama ve İzleme:**
        - Prometheus, zaman serisi verilerini toplar ve depolar. Örneğin, CPU kullanımı, bellek kullanımı, disk I/O ve network trafiği gibi metrikler sürekli olarak izlenir.
        - Bu metrikler Prometheus Query Language (PromQL) kullanılarak sorgulanabilir.
    **Prometheus Monitoring Yapılandırma (YAML):**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  labels:
	    app: prometheus
	  name: prometheus
	spec:
	  containers:
	  - name: prometheus
	    image: prom/prometheus
	    ports:
	    - containerPort: 9090
	    volumeMounts:
	    - name: config-volume
	      mountPath: /etc/prometheus
	    - name: data-volume
	      mountPath: /prometheus
	  volumes:
	  - name: config-volume
	    configMap:
	      name: prometheus-config
	  - name: data-volume
	    emptyDir: {}
	
	```
    
    - **Grafana ile Görselleştirme:**
        - Grafana, Prometheus gibi monitoring araçlarından veri çekerek bu metrikleri grafikler ve panolar halinde sunar. Grafana ile sistem performansını anlık olarak izlemek mümkündür.
6. **Logging ve Monitoring Araçlarının Entegrasyonu:**
    **Logging:**
    - Fluentd veya Logstash gibi araçlar, container loglarını toplar ve Elasticsearch gibi bir veritabanına gönderir. Kibana ile bu loglar görselleştirilip analiz edilebilir.
    **Monitoring:**
    - Prometheus, container’lardan topladığı metrikleri Grafana'ya gönderir. Grafana bu verileri kullanarak görsel raporlar oluşturur. Ayrıca Prometheus ile Alertmanager entegrasyonu sayesinde kritik olaylar için bildirimler ayarlanabilir.
7. **Logging ve Monitoring İçin En İyi Uygulamalar:**
    - **Merkezi Logging ve Monitoring:** Her bir node ve Pod'dan merkezi bir yerde log ve metrik toplamak, tüm cluster’ı yönetmeyi ve sorunları tespit etmeyi kolaylaştırır.
    - **Alarmlar ve Bildirimler:** Alertmanager veya diğer araçlar ile kritik olaylar (örneğin, CPU veya bellek aşımı) için alarmlar ayarlayın. Bu sayede hızlıca müdahale edebilirsiniz.
    - **Log Rotasyonu ve Depolama:** Log dosyaları büyüdükçe, log rotasyonu yaparak diskin dolmasını engelleyin. Eski logları depolamak ve gerektiğinde erişmek için doğru bir strateji belirleyin.
    - **Otomatik Ölçeklendirme:** Monitoring araçları ile sistem performansını izleyerek otomatik ölçeklendirme kuralları oluşturun. Örneğin, CPU kullanımı belirli bir eşiği aşarsa, yeni Pod'lar ekleyerek iş yükünü dengeleyin.
8. **Sorun Giderme:**
    - **Loglar Eksikse:**
        - Fluentd veya Logstash yapılandırmalarını kontrol edin. Logların doğru hedefe yönlendirildiğinden emin olun. Ayrıca, log dosyalarının erişilebilir olduğundan emin olun.
    - **Metrik Toplanmıyorsa:**
        - Prometheus veya cAdvisor gibi araçların doğru şekilde çalıştığından emin olun. `kubectl logs` ile Prometheus loglarını kontrol edin. Network veya yetkilendirme sorunları olup olmadığını denetleyin.
    - **Gecikme veya Performans Sorunları:**
        - Prometheus’un sorgu performansını izleyin ve veri toplama aralıklarını optimize edin. Gereksiz veri toplama işlemlerini azaltarak Prometheus'un performansını artırabilirsiniz.
### Rolling Updates ve Rollbacks Notları
1. **Rolling Updates Nedir?**
    - **Tanım:** Kubernetes’te **Rolling Update**, bir uygulamanın yeni bir sürümünü dağıtırken mevcut Pod'ları kesintiye uğratmadan yavaş yavaş güncelleyerek yerine yeni Pod'ları yerleştirme işlemidir.
    - **Görev:** Uygulamanın tüm Pod'larını aynı anda yeniden başlatmadan, belirli bir sırayla yeni Pod'ları devreye alarak kesintisiz hizmet sağlamayı amaçlar. Bu sayede uygulamanın sürekli erişilebilir olması sağlanır.
2. **Rolling Updates Nasıl Çalışır?**
    - **Adım Adım Güncelleme:** Eski Pod'lar aşamalı olarak kapatılır ve yerlerine yeni Pod'lar başlatılır. Bu süreç, belirtilen **maxUnavailable** ve **maxSurge** parametrelerine göre kontrol edilir.
    - **Kesintisiz Hizmet:** Pod'ların hepsi aynı anda kapatılmadığından, hizmet kesintisi yaşanmaz. Bu, özellikle yüksek erişim gerektiren uygulamalar için kritiktir.
3. **Rolling Update Parametreleri:**
    - **maxUnavailable:** Güncelleme sırasında aynı anda kaç Pod’un kapatılabileceğini kontrol eder. Genellikle bir yüzde veya sayı olarak belirtilir.
    - **maxSurge:** Aynı anda eklenebilecek yeni Pod sayısını belirler. Bu, mevcut Pod sayısına ek olarak çalıştırılacak yeni Pod'ları ifade eder.
    
    **Örnek: Rolling Update Konfigürasyonu (YAML)**

	```yaml
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: nginx-deployment
	spec:
	  replicas: 3
	  strategy:
	    type: RollingUpdate
	    rollingUpdate:
	      maxUnavailable: 1
	      maxSurge: 1
	  template:
	    metadata:
	      labels:
	        app: nginx
	    spec:
	      containers:
	      - name: nginx
	        image: nginx:1.17
	
	```
    **Açıklama:**
    - **maxUnavailable: 1:** Güncelleme sırasında aynı anda en fazla 1 Pod’un kapatılmasına izin verilir.
    - **maxSurge: 1:** Aynı anda en fazla 1 yeni Pod çalıştırılabilir.
4. **Rolling Updates Komutları:**
    - **Güncelleme Başlatma:** Yeni bir imaj ile Deployment güncellemesi başlatmak için `kubectl set image` komutu kullanılır.
    - **Güncelleme Durumunu İzleme:** Güncelleme sürecini izlemek için `kubectl rollout status` komutu kullanılır.
    **Örnek Komutlar:**
    
```bash
# Bir Deployment üzerinde güncelleme başlat
kubectl set image deployment/nginx-deployment nginx=nginx:1.18

# Güncellemenin ilerlemesini izle
kubectl rollout status deployment/nginx-deployment
```
    
5. **Rolling Update’in Avantajları:**
    - **Kesintisiz Dağıtım:** Uygulama Pod'ları kademeli olarak güncellendiği için hizmet kesintisi olmadan yeni sürüm devreye alınabilir.
    - **Hata Toleransı:** Güncelleme sırasında bir sorun olursa, sürecin tamamlanmadan önce fark edilip durdurulması mümkündür.
    - **Dinamik Ölçekleme:** Uygulama trafiği sürekli olarak Pod'lar arasında paylaştırıldığından, trafik akışı kesintiye uğramaz.
6. **Rollback Nedir?**
    - **Tanım:** **Rollback**, Kubernetes’te yapılan bir güncelleme sırasında veya sonrasında bir sorunla karşılaşıldığında, uygulamanın önceki başarılı sürümüne geri döndürülme işlemidir.
    - **Görev:** Eğer bir güncelleme sırasında hatalar oluşursa, önceki sürüme dönerek uygulamanın stabil bir durumda kalmasını sağlar. Bu işlem, hızlı bir şekilde sistemi kurtarmak için kritik öneme sahiptir.
7. **Rollback Nasıl Yapılır?**
    - Kubernetes, Deployment’ların geçmiş sürümlerini saklar ve bu sürümlere geri dönme imkânı sunar. Güncellemede sorun olduğunda, `kubectl rollout undo` komutuyla önceki sürüme hızlıca geri dönebilirsiniz.
    
    **Örnek: Rollback Komutları**
    
	```yaml 
	# Mevcut bir Deployment'ı geri al
	kubectl rollout undo deployment/nginx-deployment
	```
    
    - **rollback-to-revision:** Belirli bir sürüme dönmek için `--to-revision` seçeneği ile rollback yapılabilir.
    
	```yaml
	kubectl rollout undo deployment/nginx-deployment --to-revision=2
	```
    
8. **Rollback’in Avantajları:**
    - **Hızlı Kurtarma:** Güncelleme sırasında bir sorun çıkarsa, uygulamayı hızla önceki başarılı duruma geri döndürebilirsiniz.
    - **Otomatik Sürüm Saklama:** Kubernetes, Deployment güncellemelerini otomatik olarak saklar, böylece manuel olarak önceki sürümleri takip etmek zorunda kalmazsınız.
9. **Rolling Update ve Rollback En İyi Uygulamalar:**
    - **Küçük Güncellemeler Yapın:** Daha küçük ve kontrollü güncellemeler, olası hataları daha erken tespit etmeye yardımcı olur.
    - **Blue/Green veya Canary Dağıtımları:** Daha kritik güncellemelerde tüm Pod'ları aynı anda güncellemek yerine, önce bir kısmını güncelleyerek yeni sürümün performansını test edebilirsiniz.
    - **Monitoring ve Alerting:** Rolling updates sırasında Prometheus ve Grafana gibi monitoring araçlarını kullanarak, olası performans sorunlarını erken tespit edebilirsiniz.
    - **Otomatik Rollback Kuralları:** Eğer belirli metriklerde (örneğin, yüksek hata oranı) bir bozulma olursa, otomatik rollback mekanizmaları ayarlayarak uygulamanın önceki stabil sürüme dönmesini sağlayabilirsiniz.
10. **Sorun Giderme:**
    - **Güncelleme Tamamlanmadıysa:**
        - **Nedeni:** Yetersiz kaynaklar, hatalı image veya yanlış yapılandırma olabilir.
        - **Çözüm:** Güncelleme sürecini `kubectl rollout status` komutu ile izleyin ve `kubectl describe` ile daha fazla bilgi edinin.
    - **Rollback Yapılmıyorsa:**
        - **Nedeni:** Belirli bir sürümde hata olabilir veya önceki sürüm kaybolmuş olabilir.
        - **Çözüm:** `kubectl rollout history` komutuyla sürüm geçmişini kontrol edin ve doğru sürüme geri döndüğünüzden emin olun.
### Commands ve Arguments Notları
1. **Commands ve Arguments Nedir?**
    - **Commands:** Kubernetes’te bir container’ın başlatıldığında çalıştıracağı temel komutu (entrypoint) tanımlar. Bu komut, container’ın yaşam döngüsü boyunca çalışan ana işlemdir.
    - **Arguments:** Container’ı başlatırken kullanılan komuta eklenen argümanları tanımlar. Bu argümanlar, command ile birlikte çalışarak container’ın nasıl davranacağını belirler.
2. **Commands ve Arguments Nasıl Kullanılır?**
    - Kubernetes’te container’ları başlatırken, `command` (veya `entrypoint`) ve `args` (veya `arguments`) kullanarak container’ın çalıştıracağı komutları özelleştirebilirsiniz.
    - Eğer `command` veya `args` belirtilmezse, container imajında tanımlanan varsayılan komut ve argümanlar kullanılır.
3. **Command ve Arguments Arasındaki Fark:**
    - **Command (Entrypoint):** Container’ın çalıştırdığı temel komut veya giriş noktasıdır. Bu, container’ın başlatıldığında hangi işlemi yapacağını belirler.
    - **Arguments (Args):** Command (entrypoint) tarafından çalıştırılan komuta verilen ek parametrelerdir. Bu parametreler komutun nasıl çalışacağını kontrol eder.
4. **Command ve Arguments Tanımları (YAML):**
    **Örnek 1: Basit Command ve Arguments Kullanımı**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: example-container
	    image: busybox
	    command: ["echo"]
	    args: ["Hello, Kubernetes!"]
	```
    
    **Açıklama:**
    - **command:** Container başlatıldığında çalıştırılacak komut `echo` olarak belirtilmiş.
    - **args:** `echo` komutuna argüman olarak "Hello, Kubernetes!" iletilmiş. Bu container çalıştırıldığında konsolda bu mesaj görüntülenecek.
    
    **Örnek 2: Command ve Arguments ile Özelleştirilmiş Bir Komut Çalıştırma**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: example-container
	    image: nginx
	    command: ["/bin/sh"]
	    args: ["-c", "echo Hello && sleep 3600"]
	```
    
    **Açıklama:**
    - **command:** Container’ın giriş noktası `/bin/sh` olarak tanımlanmış. Bu, bir kabuk komutunun çalıştırılacağını belirtir.
    - **args:** `-c` komutuna ek olarak, kabuk komutları argüman olarak verilmiş. Bu durumda, container "Hello" yazdıktan sonra 3600 saniye uyuyacak.
5. **Command ve Args Kullanımında Dikkat Edilmesi Gerekenler:**
    - **Varsayılan Değerler:** Eğer `command` ve `args` belirtilmezse, container imajında tanımlı varsayılan giriş noktası ve argümanlar çalıştırılır. Dockerfile’daki `ENTRYPOINT` ve `CMD` talimatlarıyla bu ayarlar yapılır.
    - **Command ve Args’ın Kombinasyonu:** `command` ile container'ın giriş noktası belirlenir ve `args` ile bu komutun nasıl çalışacağına dair detaylar verilir. Bu iki alan birlikte çalışır ve container’ın davranışını belirler.
    - **Container Bağlamına Dikkat:** Her container’ın bir giriş komutu vardır. Bu komutlar, container’ın çalıştırılacağı ortam ve işlevselliğe göre ayarlanmalıdır. Örneğin, bir veri tabanı container’ında `command` alanını kullanarak başlatma komutları özelleştirilebilir.
6. **Gelişmiş Command ve Arguments Kullanımı:**
    - **Birden Fazla Argüman:** Eğer birden fazla argüman iletmek istiyorsanız, `args` kısmına bir liste halinde ekleyebilirsiniz.
    - **Komut Zinciri:** `args` ile birden fazla komut çalıştırılabilir ve bunlar `&&` gibi operatörlerle zincirlenebilir.
    **Örnek:**
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: multi-command-pod
	spec:
	  containers:
	  - name: busybox-container
	    image: busybox
	    command: ["/bin/sh"]
	    args: ["-c", "echo Starting... && sleep 5 && echo Done!"]
	```
    **Açıklama:**
    - **command:** `/bin/sh` çalıştırılır.
    - **args:** Bu örnekte, komutlar zincirlenmiştir. Önce "Starting..." mesajı gösterilir, ardından 5 saniye beklenir ve sonra "Done!" mesajı görüntülenir.
7. **Varsayılan Komutu Değiştirme:**
    - Docker imajında tanımlanmış varsayılan bir `command` veya `args` olabilir. Kubernetes’te bunları geçersiz kılmak için kendi `command` ve `args` değerlerinizi tanımlayabilirsiniz.
    - Eğer sadece argümanları değiştirmek isterseniz, `command` kısmını boş bırakıp sadece `args` belirtebilirsiniz. Bu durumda imajın varsayılan `command` değeri çalışmaya devam eder.
8. **Common Use Cases (Yaygın Kullanım Senaryoları):**
    - **Custom Commands:** Uygulamanın başlatılmadan önce belirli işlemler yapması gerektiğinde (örneğin, bir ön hazırlık komutu çalıştırmak), `command` ve `args` kullanarak uygulamanın başlatılma şeklini özelleştirebilirsiniz.
    - **Task Containers:** Container’ın yalnızca belirli bir görevi (örn. bir yedekleme işlemi, dosya kopyalama vb.) gerçekleştirmek için çalıştırılması gerektiğinde, `command` ve `args` ile container’ı sadece bu görev için başlatabilirsiniz.
    - **Debugging:** Container’ı normal işleyişinden farklı bir şekilde başlatıp, örneğin, `sh` kabuğu ile container içine erişim sağlamak gibi durumlarda kullanılabilir.
    **Örnek: Pod Başlangıcında Kabuk (Shell) ile Debugging**
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: debug-pod
	spec:
	  containers:
	  - name: busybox-container
	    image: busybox
	    command: ["/bin/sh"]
	    args: ["-c", "while true; do echo 'Debugging...'; sleep 10; done"]
	```
    **Açıklama:**
    - **command:** `/bin/sh` kabuğu çalıştırılıyor.
    - **args:** Her 10 saniyede bir "Debugging..." mesajı yazdırılıyor.
9. **Sorun Giderme:**
    - **Command veya Args Yanlışsa:**
        - **Nedeni:** Komutlar doğru yapılandırılmamış olabilir.
        - **Çözüm:** Pod’un loglarına `kubectl logs <pod-name>` ile erişip komutların doğru çalışıp çalışmadığını kontrol edin. YAML yapılandırmasındaki command ve args değerlerini gözden geçirin.
    - **Varsayılan Komutlar Değiştirilemiyorsa:**
        - **Nedeni:** Dockerfile’da tanımlanan `ENTRYPOINT` ve `CMD` ayarları olabilir.
        - **Çözüm:** Kubernetes YAML dosyasında `command` alanını doğru şekilde yapılandırarak bu varsayılan ayarları geçersiz kılabilirsiniz.
### Ortam Değişkenleri Konfigürasyonu ve ConfigMaps Notları
1. **Ortam Değişkenleri (Environment Variables) Nedir?**
    - **Tanım:** Kubernetes'te, container'lara dış kaynaklı bilgileri (örneğin, yapılandırma ayarları) geçirmek için kullanılan bir yöntemdir. Ortam değişkenleri, container’ların başlatılması sırasında belirlenen anahtar-değer çiftleridir ve uygulamaların çalıştırılma şeklini kontrol etmek için kullanılır.
    - **Görev:** Container’ların yapılandırmalarını dinamik olarak ayarlamak için kullanılır. Bu sayede container imajı değiştirilmeden, ortam değişkenleri üzerinden yapılandırma ayarları yapılabilir.
2. **Ortam Değişkenleri Nasıl Kullanılır?**
    - Kubernetes’te ortam değişkenleri, Pod tanımının `env` alanında belirtilir. Ortam değişkenleri sabit bir değer alabilir ya da başka kaynaklardan (ConfigMap, Secret gibi) alınabilir.
    **Basit Bir Ortam Değişkeni Tanımı (YAML):**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: example-container
	    image: nginx
	    env:
	    - name: EXAMPLE_ENV_VAR
	      value: "Hello Kubernetes!"
	```
    
    **Açıklama:**
    - **env:** Pod tanımındaki `env` alanı ile ortam değişkenleri tanımlanır.
    - **name:** Ortam değişkeninin adı (`EXAMPLE_ENV_VAR`).
    - **value:** Bu değişkenin alacağı değer (`"Hello Kubernetes!"`).
3. **ConfigMaps Nedir?**
    - **Tanım:** ConfigMap, Kubernetes’te yapılandırma verilerini (anahtar-değer çiftleri) tutan bir kaynaktır. ConfigMap’ler, container’ların yapılandırma ayarlarını dış kaynaklı olarak almasını sağlar.
    - **Görev:** Uygulama yapılandırmalarını container imajından ayırarak, imajı değiştirmeden yapılandırma güncellemeleri yapmanıza olanak tanır. Bu, uygulamaları daha esnek ve yönetilebilir hale getirir.
4. **ConfigMap Nasıl Kullanılır?**
    - **ConfigMap Oluşturma:** ConfigMap’ler `kubectl create configmap` komutu ile ya da YAML dosyaları aracılığıyla oluşturulabilir.
    
    **ConfigMap Tanımı (YAML):**
    
	```yaml
	apiVersion: v1
	kind: ConfigMap
	metadata:
	  name: example-configmap
	data:
	  APP_ENV: "production"
	  LOG_LEVEL: "info"
	```
    
    **Açıklama:**
    - **data:** ConfigMap içinde tanımlanan anahtar-değer çiftleridir. Bu örnekte, `APP_ENV` ve `LOG_LEVEL` isimli iki yapılandırma değişkeni tanımlanmıştır.
5. **ConfigMap ve Ortam Değişkenlerinin Entegrasyonu:**
    - ConfigMap, bir Pod içinde ortam değişkeni olarak kullanılabilir. Böylece, ConfigMap’teki veriler, container'lara otomatik olarak ortam değişkeni olarak atanabilir.
    **ConfigMap Kullanarak Ortam Değişkeni Tanımı (YAML):**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: example-container
	    image: nginx
	    env:
	    - name: APP_ENV
	      valueFrom:
	        configMapKeyRef:
	          name: example-configmap
	          key: APP_ENV
	```
    
    **Açıklama:**
    - **valueFrom:** Bu alan, ortam değişkeninin değerini ConfigMap’ten alacağını belirtir.
    - **configMapKeyRef:** Ortam değişkeninin alındığı ConfigMap’i ve anahtarı belirtir.
    - **name:** ConfigMap’in adı (`example-configmap`).
    - **key:** ConfigMap içinde kullanılan anahtar (`APP_ENV`).
6. **Tüm ConfigMap Değerlerini Ortam Değişkeni Olarak Geçirme:**
    - Bir Pod içinde birden fazla ConfigMap anahtarını tek tek tanımlamak yerine, tüm ConfigMap değerlerini birden ortam değişkeni olarak geçirebilirsiniz.
    **Tüm ConfigMap’i Ortam Değişkeni Olarak Kullanma (YAML):**
    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: example-container
	    image: nginx
	    envFrom:
	    - configMapRef:
	        name: example-configmap
	```
    
    **Açıklama:**
    - **envFrom:** Bu alan, belirtilen ConfigMap’teki tüm anahtar-değer çiftlerinin ortam değişkeni olarak kullanılmasını sağlar.
    - **configMapRef:** ConfigMap’in adı belirtilir. Bu örnekte, `example-configmap`teki tüm anahtar-değer çiftleri container’a ortam değişkeni olarak atanır.
7. **ConfigMap’i Dosya Olarak Kullanma:**
    - ConfigMap, sadece ortam değişkeni olarak değil, bir dosya olarak da container içinde kullanılabilir. Bu yöntem, daha büyük yapılandırma dosyaları için kullanışlıdır.
    
    **ConfigMap’i Dosya Olarak Mount Etme (YAML):**
    
	```ymal
	apiVersion: v1
	kind: Pod
	metadata:
	  name: example-pod
	spec:
	  containers:
	  - name: example-container
	    image: nginx
	    volumeMounts:
	    - name: config-volume
	      mountPath: /etc/config
	  volumes:
	  - name: config-volume
	    configMap:
	      name: example-configmap
	```
    
    **Açıklama:**
    - **volumeMounts:** ConfigMap, container içinde belirli bir dizine mount edilir. Bu örnekte, `/etc/config` dizinine ConfigMap’teki veriler dosya olarak yerleştirilecek.
    - **volumes:** Container'a mount edilen ConfigMap burada belirtilir. `example-configmap` ConfigMap'i, container’ın dosya sistemi içine monte edilir.
8. **ConfigMap ve Secret Arasındaki Fark:**
    - **ConfigMap:** Genel yapılandırma verilerini içerir. Veriler şifrelenmemiş olarak saklanır.
    - **Secret:** Hassas bilgileri (örn. şifreler, API anahtarları) saklamak için kullanılır ve veriler şifreli olarak tutulur. ConfigMap’ten farklı olarak daha güvenlidir ve hassas bilgiler için kullanılması önerilir.
9. **Ortam Değişkenleri ve ConfigMap En İyi Uygulamalar:**
    - **Yapılandırmayı Uygulamadan Ayırın:** ConfigMap kullanarak uygulamaların yapılandırma ayarlarını imajlardan ayırın. Bu, uygulamayı yeniden inşa etmeden yapılandırma ayarlarını değiştirmenizi sağlar.
    - **ConfigMap’leri Doğru Yönetin:** ConfigMap’ler, uygulama yapılandırmalarını tutmak için merkezi bir kaynak olabilir. Bu nedenle ConfigMap’lerin doğru şekilde version control ile yönetilmesi önemlidir.
    - **Secrets Kullanımını Düşünün:** Eğer ortam değişkenleri hassas bilgiler içeriyorsa, bunları ConfigMap yerine Secret kullanarak yönetin. Bu, verilerin daha güvenli olmasını sağlar.
    - **Ortam Değişkenleri ile ConfigMap’i Birleştirin:** Özelleştirilmiş uygulama ayarlarını dinamik olarak yönetmek için ConfigMap ve ortam değişkenlerini birlikte kullanın.
10. **Sorun Giderme:**
    - **Ortam Değişkeni Tanımlanmamışsa:**
        - **Nedeni:** ConfigMap’te yanlış anahtar tanımlanmış veya Pod tanımında yanlış yapılandırma yapılmış olabilir.
        - **Çözüm:** `kubectl describe pod <pod-name>` komutuyla Pod’u inceleyin ve ConfigMap anahtarlarının doğru olup olmadığını kontrol edin.
    - **ConfigMap Güncellemeleri Uygulanmıyorsa:**
        - **Nedeni:** ConfigMap güncellendikten sonra mevcut Pod'lar yeni ayarları otomatik olarak almaz. Pod'un yeniden başlatılması gerekebilir.
        - **Çözüm:** Pod’ları yeniden başlatarak ConfigMap’in güncellenmiş değerlerinin uygulanmasını sağlayın.
### Kubernetes Secrets Notları
1. **Secrets Nedir?**
    - **Tanım:** Kubernetes’te **Secrets**, hassas bilgileri (örneğin, şifreler, API anahtarları, tokenlar) güvenli bir şekilde saklamak ve container'lara bu bilgileri güvenli bir yolla sağlamak için kullanılan bir kaynaktır. Secrets, ConfigMap’e benzer şekilde çalışır, ancak veri şifreli olarak tutulur ve erişim kontrollüdür.
    - **Görev:** Secrets, hassas bilgilerin imaj dosyalarına veya Pod tanımlarına sabitlenmesini önleyerek, güvenli bir yapılandırma yönetimi sağlar.
2. **Secrets Nasıl Çalışır?**
    - Secrets, Kubernetes API server tarafından şifrelenir ve `base64` ile kodlanarak saklanır. Bu bilgiler, container’lara ortam değişkeni olarak ya da dosya sistemi üzerinden erişilebilir.
3. **Secrets Türleri:**
    - **Opaque (Şifrelenmiş):** En yaygın kullanılan secret türüdür ve kullanıcı tarafından oluşturulan key-value çiftlerini içerir.
    - **docker-registry:** Docker Hub veya diğer container registry'lerine kimlik doğrulaması yapmak için kullanılır.
    - **TLS:** SSL/TLS sertifikalarını saklamak için kullanılır.
    - **Service Account Token:** Kubernetes içindeki servis hesaplarına token olarak atanmış secrets.
4. **Secret Nasıl Oluşturulur?**
    - Secrets, `kubectl create secret` komutuyla veya YAML dosyası ile oluşturulabilir.
    **Komut ile Secret Oluşturma:**
	```bash
	kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=admin123
	```
    **YAML ile Secret Tanımı:**
	```yaml
	apiVersion: v1
	kind: Secret
	metadata:
	  name: my-secret
	type: Opaque
	data:
	  username: YWRtaW4=   # base64 ile kodlanmış "admin"
	  password: YWRtaW4xMjM=   # base64 ile kodlanmış "admin123"
	```
    **Açıklama:**
    - **data:** Secrets verileri `base64` formatında kodlanmış olarak saklanır. `kubectl` komutları veya diğer yöntemler ile bu veriler kullanılabilir.
    - **type:** Secret’ın türünü belirtir. `Opaque` genel amaçlı secrets için kullanılır.
    
    **Veriyi Base64 Formatına Çevirme (Yardımcı Komut):**
	```bash
	echo -n 'admin' | base64
	```
    Bu komut, veriyi base64 formatına çevirir. Secrets YAML dosyasında kullanılmak üzere bu komutlarla şifrelenmiş veriler elde edilir.
5. **Secrets Kullanımı:**
    **Ortam Değişkeni Olarak Secret Kullanma (YAML):**
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: secret-env-pod
	spec:
	  containers:
	  - name: example-container
	    image: nginx
	    env:
	    - name: USERNAME
	      valueFrom:
	        secretKeyRef:
	          name: my-secret
	          key: username
	    - name: PASSWORD
	      valueFrom:
	        secretKeyRef:
	          name: my-secret
	          key: password
	```
    **Açıklama:**
    - **env:** Pod tanımında ortam değişkenleri `secretKeyRef` ile belirtilir. Bu alan, secret’tan gelen değerleri ortam değişkeni olarak container’a aktarır.
    - **secretKeyRef:** Secret'ın adı (`my-secret`) ve key değeri (`username`, `password`) ile secret verilerini container’a yükler.
    **Dosya Olarak Secret Kullanma (YAML):**
    
	```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-volume-pod
spec:
  containers:
  - name: example-container
    image: nginx
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/secret-data
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: my-secret
	```
    **Açıklama:**
    - **volumeMounts:** Secret, Pod’un dosya sistemi içinde belirli bir dizine (`/etc/secret-data`) monte edilir. Container, secret içeriğini dosya olarak kullanabilir.
    - **volumes:** Secret verisi, volume olarak mount edilir ve container içinde dosya olarak erişilebilir.
6. **Docker Registry için Secret Kullanma:**
    Eğer özel bir container registry’den imaj çekmek isterseniz, kimlik doğrulaması için secrets kullanabilirsiniz.
    **Docker Registry Secret Oluşturma:**
    ```bash
  kubectl create secret docker-registry my-registry-secret \
  --docker-username=<your-username> \
  --docker-password=<your-password> \
  --docker-email=<your-email>  
```
    **Kullanımı (YAML):**
	```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-reg-pod
spec:
  containers:
  - name: example-container
    image: my-private-registry/nginx
  imagePullSecrets:
  - name: my-registry-secret
```
    **Açıklama:**
    - **imagePullSecrets:** Pod, Docker registry’den imaj çekmek için `my-registry-secret` secret’ını kullanır.
7. **Secrets ve ConfigMaps Arasındaki Farklar:**
    - **ConfigMaps:** Şifrelenmemiş genel yapılandırma verilerini tutar.
    - **Secrets:** Hassas bilgileri şifreli bir şekilde saklar ve bu bilgilerin güvenli bir şekilde erişilmesini sağlar.
8. **Secrets Güvenliği:**
    - **Encryption at Rest:** Kubernetes, Secret verilerini disk üzerinde şifrelenmiş bir şekilde saklar. Bu, verilerin düz metin olarak saklanmasını önler.
    - **RBAC ile Erişim Kontrolü:** Secrets verilerine erişim, Role-Based Access Control (RBAC) ile sınırlanabilir. Yalnızca belirli kullanıcılar veya Pod’lar, Secret’lara erişebilir.
    - **Sağlama:** Secrets verileri, container’lara yalnızca ihtiyaç duyulduğunda sağlanır. Bu, hassas bilgilerin gereksiz yere yayılmasını önler.
9. **Secrets En İyi Uygulamalar:**
    - **Hassas Bilgileri Secrets ile Yönetin:** Uygulama şifreleri, API anahtarları ve diğer hassas bilgileri kesinlikle ConfigMap yerine Secrets kullanarak yönetin.
    - **RBAC ile Güvenliği Sağlayın:** Secrets’a yalnızca gerekli rollerin ve kullanıcıların eriştiğinden emin olun.
    - **Secrets Güncellemelerini Otomatikleştirin:** Eğer bir secret güncellenirse, Pod’ların otomatik olarak bu değişiklikleri almasını sağlamak için doğru volume montaj ve ortam değişkeni yapılandırmalarını kullanın.
    - **Secrets’i Şifreleyin:** Disk üzerinde saklanan secret verilerinin Kubernetes’te "Encryption at Rest" özelliği ile şifrelendiğinden emin olun.
10. **Sorun Giderme:**
    - **Secret Kullanımı Başarısız Olursa:**
        - **Nedeni:** Pod tanımında secret adı veya anahtarı yanlış olabilir ya da RBAC izinleri eksik olabilir.
        - **Çözüm:** `kubectl describe pod <pod-name>` komutu ile Pod detaylarını inceleyin. Secrets’ın doğru tanımlandığını ve erişim izinlerinin uygun olduğunu kontrol edin.
    - **Secret Verileri Eksikse:**
        - **Nedeni:** Secret tanımında base64 kodlaması yanlış olabilir.
        - **Çözüm:** Secret verilerini `base64` ile doğru kodladığınızdan emin olun. Kodlamayı kontrol etmek için şu komutu kullanabilirsiniz:
        ```bash
        echo -n 'admin' | base64
```
