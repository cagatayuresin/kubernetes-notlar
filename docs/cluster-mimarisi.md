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
