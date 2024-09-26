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
