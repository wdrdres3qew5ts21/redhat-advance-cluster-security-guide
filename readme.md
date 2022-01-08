## Red Hat Advanced Cluster Security
ปัจจุบันการ Deploy Application ในรูปแบบของ Container นั้นเป็นรูปแบบทีไ่ด้รับความนิยมสำหรับ Project ที่ต้องการคุณสมบัติการ Scale ที่ยืดหยุ่นสามารถ Deploy ได้ง่ายไม่ยึดติดกับ Host หรือ Virtual Machine ที่ทำงานอยู่ ซึ่งด้วยความสะดวกของการ Scale Application ในรูปแบบของ Container ก็ทำให้เรามี Application ที่ถูก Scale Out ออกไปหลายๆแอพและไม่จำเป็นต้องอยู่ในเครื่องเดียวกันก็ได้ แต่กระนั้นเองก็อาจจะเป็นการเพิ่ม Attack Surface ได้เช่นเดียวกันเพราะ Application ไมไ่ด้อยู่แค่ที่ Host Machine เครื่องเดียวอีกต่อไป ซึ่งถ้าหาก Application ที่เรา Deploy ไปนั้นมีช่องโหว่ Vulnerability ในหลายๆที่หลายเครื่อง Machine ตามไปด้วย แต่ไม่ใช่หมายความว่าการ Scale Out Application เป็นสิ่งไม่ดีนะ TwT  เพียงแต่เป็นเรื่องความจริงของ Application ที่อยู่ในรูปแบบ Distributed อยู่แล้วนั่นเองคับผม  ซึ่งก่อนส่งท้ายปี 2021 ที่ผ่านมาเราคงจะจำอภิมหากาฬช่องโหว่ Vulnerability อย่าง Log4Shell ที่อยู่ใน Library ของ Log4J ใน Java นั่นเองซึ่งก็กระทบแทบจะเกือบทุก Application ในแพลตฟอร์มใหญ่ๆที่เราใช้งานทั้งหมดซึ่งสำหรับวิธีการแก้ไขที่ดีที่สุดเลยก็คือการที่เรารีบอัพเกรด Library ของเราให้ไปยัง Version ล่าสุดที่ผ่านการแก้ไขช่องโหว่นี้เรียบร้อยแล้วกับคอยตามช่องโหว่อยู่ตลอดเวลา ซึ่งความยิ่งใหญ่นี้ถึงขนาดที่เว็บไซต์อย่าง Docker Hub ที่ใช้สำหรับฝาก Container Image นั้นมีขึ้นแจ้งเตือน Log4Shell ไว้ที่ตรง Tag ของ Image ที่จะ Pull เลยทีเดียว ซึ่งก็เป็นการช่วยให้เราอุ่นใจได้มากขึ้นสำหรับ Image ใหม่ๆที่เราจะ Pull ลงมาแต่ถ้าหากเราใช้ Private Registry ทั่วๆไปที่เราใช้ในการฝาก Container Image เฉยๆหรือว่า Image ของเรานั้นถูกรันทำงานใน Cluster ไปตั้งนานแล้วเราก็คงจะไม่ทราบว่าแล้ว Image ที่กำลังทำงานอยู่นั้นปลอดภัยดีหรือเปล่าจากช่องโหว่ Log4Shell 

ซึ่งวันนี้เองเราจะลองมาทดลองใช้ Product  Red Hat Advance Cluster Security ซึ่งเป็นส่วนของ Project Opensource ชื่อว่า Open Cluster Management ซึ่งตอนนี้กำลังเป็น Sandbox Project ของ Cloud Native Computing Foundation https://open-cluster-management.io 

![redhat-advance-cluster-security](images/intro/banner-open-cluster.png)

ซึ่งไว้เดี่ยวครั้งหน้าเราจะลองมาใช้ Project Open Cluster Management ในการควบคุม Deploy Application ไปหลายๆ Cluster พร้อมกับ Rule เงื่อนไขการ Deploy กันคับผม ซึ่งสำหรับวันนี้ส่วนที่เราจะลองมาเดโม่กันคือส่วนของ Advance Cluster Security ที่ลิ้งค์นี้ https://github.com/stolostron/advanced-cluster-security แต่สำหรับ Demo ครั้งนี้เราจะใช้ Operator ที่อยู่ใน Openshift Cluster ในการติดตั้ง Product Red Hat Advance Cluster Security ซึ่งเป็น Version ที่มี Subscription และได้รับการ Support การใช้งานดูแลจาก Red Hat กันว่าถ้าอยากลอง Config ใช้งานแบบง่ายๆสำหรับสร้าง Rule กฏสำหรับการตรวจจับ Log4Shell และช่องโหว่ Security อื่นๆต้องทำอย่างไรบ้าง
โดย Red Hat Advance Cluster Security นั้นสามารถติดตั้งได้ทั้งใน Vanila Kubernetes และ Openshift เองเพียงแต่ว่าสำหรับ GUI Console ที่เป็นเสมือนศููนย์ในการควบคุม Rule และกฏในการ Secure Cluster ถ้าจะลงก็จะลงในตั้งแต่ Openshift 4.5 เป็นต้นไปก็จะ Support แบบเต็มรูปแบบมากที่สุด https://docs.openshift.com/acs/3.67/installing/prerequisites.html ส่วน Cluster ปลายทางที่ต้องเราต้องการ Protect โดยใช้ Rule ที่สั่งมาจาก Central GUI ที่เราจะเรียก Component นี้ว่า SecuredCluster โดยสามารถไปดูรายละเอียดเพิ่มเติมได้ที่ https://access.redhat.com/node/5822721

![redhat-advance-cluster-security](images/intro/support-matrix.png)

### Advance Cluster Security ทำงานได้อย่างไรและใช้เทคนิคอะไรเบื้องหลังกันนะ ?
สำหรับตัวอย่างที่เราจะมาทดลองกันในวันนี้จะประกอบไปด้วยของตรวจจับ CVE ช่องโหว่ของ Log4Shell โดยตรงและตรวจจับ Pod ที่มีพฤติกรรมการทำงานแล้วขัดกับกฏที่เราตั้งเอาไว้ใน Cluster อย่างเช่นห้ามมี User ใช้คำสั่งเข้าไปใน Pod โดยตรงเพื่อความปลอดภัยเพราะบาง Pod ก็อาจจะมี ServiceAccount ที่มี Credentials สามารถต่อตรงไปหา Kube API Server ได้อย่างเช่น Pod ที่ใช้ใน Pipeline CI/CD แล้วเราสั่งให้ Pod นั้นสามารถใช้คำสั่ง kubectl หรือเชื่อมไปหา Kube API Server แล้วเกิดเราไม่ได้จำกัดสิทธิจะกลายเป็นว่า Pod ใน Pipeline CI/CD จะกลายเป้นจุดที่น่ากลัวมากเพราะ Pod CI/CD ของเราอาจจะไปลบหรือแอบไป Deploy อะไรก็ได้จาก User หรือใครที่เข้ามาถึง Pod นี้ได้ ดังนั้นเพื่อความปลอดภัยเราก็อาจจะห้ามมีการ exec เข้าไปใน Pod โดยตรงเลยนั่นเอง ซึ่งมาตรงถึงจุดนี้เพื่อนๆก็อาจจะสงสัยบ้างวา เอ๊แล้วมันรู้ได้ยังไงกันนะว่ามันมีอะไรเกิดขึ้นใน Cluster ของเราแล้วมันไปตรงกับกฏได้ยังไงกันนะ ? ซึ่งในปกติแล้ว Kubernetes ก็จะมี Audit Logs ให้เราสามารถเข้าไปดูเหตุการณ์ที่เกิดขึ้นใน Cluster ได้เช่นกัน https://kubernetes.io/docs/tasks/debug-application-cluster/audit และก็จะมี Admission Controller https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/ ซึ่งเป็นเสมือน Filter ตัวหนึ่งที่มาขั้นในช่วงกลางระหว่างที่ Request ของเราก่อนที่จะถูกส่งไปถึง Kubernetes API Server ก็คือ Admission Controller

![redhat-advance-cluster-security](images/intro/admission.png)

ซึ่งส่วนประกอบของตัวที่ใช้งานใน Admission Controller จะเป็น Mutating Webhook ที่ใช้ในการเปลี่ยน Config ของ Object ที่กำลังจะเกิดขึ้นมาเช่นเรามีการสร้าง Deployment ที่ไม่ได้ระบุ Resources Limit โดยปกติก็จะทำให้ Pod ใช้งาน Resources ไปเรื่อยๆตามใจก็อาจจะทำให้เกิดปัญหา Resource ไม่พอและแย่งกันใช้ได้นั่นเอง แต่ถ้าเราลองให้ Mutating Webhook บอกว่า ถ้าผู้ใช้ไม่ได้เติม Resources ของ Pod มาก็ช่วยเติมให้หน่อยนะโดย set Limit CPU กับ Memory สูงสุดที่ใช้ไว้นั่นเอง หรืออย่าง Validating Webhook ก็จะทำหน้าที่เหมือนชื่อบอกคือเช็คว่า Object ที่กำลังส่งมานั้นมันเป็นไปตามเงื่อนไขที่จะให้ตรวจสอบ Validate หรือเปล่าถ้าไม่เป็นก็แจ้ง Error บางอย่างกลับไปนั่นเอง


### eBPF (Extended Berkeley Packet Filter) หัวใจสำคัญ Monitoring ของ Linux ยุคใหม่ (รวมถึง Microsoft Windows ??!!)
สำหรับอีกคำถามหนึ่งหลังจากที่เรามีการตรวจจับหรือ Block จังหวะขาเข้ากำลังจะสร้าง (Deploy-Time) Object ไปยัง Kubernetes API Server ได้แล้วผ่าน Admission Controller เรามีเทคนิคอะไรอีกในการตรวจจับสิ่งที่ทำงานไปแล้ว (Run-time) ? 
และสำหรับการเก็บข้อมูลในจังหวะที่ Pod ของเรานั้นทำงานไปใน Cluster แล้วจะใช้เทคโนโลยีที่อยู่ใน Linux อยู่แล้วที่ชื่อว่า eBPF (Extended Berkeley Packet Filter) ซึ่งจริงๆแล้วก็เกิดมาจาก BPF (Berkeley Packet Filter) 

โดยสรุปการทำงานของโปรแกรม eBPF แบบเข้าใจง่ายคร่าวๆก็คือการที่เราเข้าใจว่า Linux นั้นมีการแบ่ง Memory เป็น Kernel และ User Space ซึ่งใน Kernel Space ก็คือจัดที่ Low Level Programming นั้นทำงานเพื่อควบคุม CPU, Memory, Drivers ต่างๆซึ่งโดยปกติแล้วผู้ใช้ทั่วๆไปไม่สามารถเข้าไปเข้าถึงส่วนนี้ได้อยู่แล้ว ส่วนใน User Space ก็คือส่วนใดๆที่ไม่ใช่ Process ที่ทำงานใน Kernel Process นั่นเองซึ่งก็จะถูกจำกัดการเข้าถึง Hardware การทำงานก็จะเป้นว่าถ้า Network ต้องการจะส่ง Packet ก็จะต้องผ่านจาก User Space ไปยัง Kernel Space และค่อยส่งไปหา Hardware อย่าง Network Card Driver ซึ่งการทำงานแบบนี้เรียกว่า System Call นั่นเอง ซึ่ง eBPF คือเทคโนโลยีปัจจุบันที่มีอยู่ใน Linux ตั้งแต่ Kernel 3.18 และถ้าอยากจะใช้แบบสมบูรณ์ต้องการอย่างน้อย Kernel 4.4 ขึ้นไป ซึ่ง eBPF นั้นทำให้ Program ที่ทำงานทั่วๆไปใน Userspace สามารถไปทำงานใน Kernel Space แบบ Bytecode ได้ซึ่งโปรแกรมที่จะทำงานได้นั้นต้องผ่านการเช็คก่อนว่ารันแล้วจะไม่ Crash ไม่อย่างนั้นก็จะทำให้ Host เราดับไปเลยนั่นเองเพราะโปรแกรมใน Kernel Space เกิด Crash ขึ้นมา และด้วยเทคนิคนี้ทำให้เราไม่จำเป็นต้อง Compile Build Linux ใหม่ทั้ง Distro ด้วยตัวเองเพื่อนำโปรแกรมไปรันใน Kernel Space ซึ่งตรงนี้คือแนวคิดคร่าวๆว่าหลังจากนี้ Program พิเศษๆที่ทำให้เราสามารถ Monitor หรือเข้าถึงใช้งาน Resource ของ Host Linux นั้นเกิดขึ้นได้อย่างไรโดยที่ทุกคนไม่ต้องไปเขียน Hardcode ลงไปใน OS แล้ว Build ใหม่ทั้ง Distro แต่สามารถเกาะผ่าน eBPF ไปได้เลยโดยตรง (หัวข้อนี้สำหรับผมเองคือเปิดมิติ Galaxy ใหม่ไปไกลมาก 55555 เพราะปกติไม่ได้ยุ่งกับ OS มากเท่าไหร่ แต่พยายามสรุปมาให้เพื่อนๆอ่านนะครับ)
สามารถอ่านเพิ่มได้ที่ https://www.infoq.com/articles/gentle-linux-ebpf-introduction/

![redhat-advance-cluster-security](images/ebpf/timeline.png)

![redhat-advance-cluster-security](images/ebpf/arch.png)

โดยในตอนแรกที่ BPF เกิดมาสำหรับการเขียน Rule ในการทำ Routing ภายในตัว Linux เองโดยถ้านึกถึงเรื่องใกล้ตัวก็คือคำสั่งที่เกี่ยวข้องกับ iptables ที่ใช้ในการทำ NAT หรือการ drop Package ระหว่าง Interface ต่างๆภายใน Linux นั่นเองซึ่งตัวอย่างที่ดังๆและน่าสนใจก็อย่างเช่น Cloud Flare ที่นำ eBPF มาผสมกับ Nftables ซึ่ง Nftables เองก็กำลังจะกลายเป็นตัวกฏ Routing หลักของ Linux ในอนาคต https://wiki.nftables.org/wiki-nftables/index.php/Moving_from_iptables_to_nftables ซึ่งเพื่อนๆสามารถไปดูได้จากเว็บของ Netfilter ที่เป็นทั้งผู้พัฒนา Nftables กับ Iptables อยู่แล้ว แต่ถึงกระนั้นเองผมก็เจอบั้คกับ Docker นะตอนไปใช้ Nftables TwT คือ Service Discovery ไม่ทำงานเลยแต่เหมือนตอนนี้จะแก้ได้แล้วล่าสุดบน Fedora35 (แต่ผมยังไม่เคยเทสเหมือนกันนะว่าแก้ได้จริงมั้ยนะ เคยแต่ลองสมัยก่อนประมาณสองปีก่อน TwT ) https://fedoramagazine.org/docker-and-fedora-35/ 

![redhat-advance-cluster-security](images/ebpf/performance.png)
ซึ่งสำหรับเหตุผลสั้นๆง่ายๆว่าทำไม Iptables มันช้าและต้องอัพเกรดมาเป็น Nftables นั่นก็คือ Iptables พออัพเดทครั้งหนึ่งมันจะต้องบังคับอัพเดทไปพร้อมๆกับ Rules ทั้งแผงกับเวลาที่มี Routing เข้ามาการ Search Rule นั้นเป็นแบบ Linear (ต้องเริ่มอ่านนับจากจุดเริม่ต้นไปจนกว่าจะเจอไม่สามารถ Jump ไปหาปลายทางแบบ Lookup หรือ Hashing นั่นเอง) ทำให้ช้ามากนั่นเองอ่านเพิ่มได้ที่จากภาพก็จะเห็นว่าพอเปลี่ยนจาก IPtables แล้วเร็วกว่าอย่างเห็นได้ชัด https://cilium.io/blog/2018/04/17/why-is-the-kernel-community-replacing-iptables


ซึ่งถ้าเป็นสมัยก่อนเราไม่มีการสร้าง Pod มากๆหรือใช้เทคนิคที่ทำงานใน Overlay Network ใน Host หนึ่งเยอะๆก็ดูจะไม่เป็นไร แต่ถ้าเราลองไปดู Routing Tables ใน Kubernetes Host ของเราจะพบว่าพวก Pod ทั้งหลายมีการแอบไปเพิ่ม Routing ไปยัง IPTables เองอัตโนมัติ
ซึ่งภาพนี้คือภาพที่ได้จากคำสั่งของ `iptables -L`
![redhat-advance-cluster-security](images/ebpf/iptables.png)

เพราะด้วยเทคนิค Overlay Network ที่เป็น Network เสมือนจำลองวิ่งบน Network ของ CNI ใน Kubernetes เองการเชื่อมต่อ Pod ใน Cluster เดียวกันเชื่อมต่อกันไปหากันได้หมดก็เลยตั้งมี Container Network Interface อย่าง Calico ที่จะเป็น DaemonSet ไปลงแล้วเกาะกับ Host Machine เพื่อทำคำสั่งที่ระดับ Low Level แบบนี้แล้วเพิ่ม Route เชื่อมเข้ากลับไปในแต่ล่ะ Pod นั่นเอง 

![redhat-advance-cluster-security](images/ebpf/veth.png)
ซึ่งเบื้องหลังก็คือการเชื่อม Virtual Ethernet Cable Interface จาก Host เสียบเข้าไปใน Pod (เหมือน Router/Switch จำลองนั่นเอง~) อ่านเพิ่มได้ที่ https://developers.redhat.com/blog/2018/10/22/introduction-to-linux-interfaces-for-virtual-networking#veth

ซึ่งภาพนี้คือภาพที่ได้จากคำสั่งของ `routel` เพื่อดูสรุปการ Routing ใน Host Linux `10.244.0.28` ของเราจะพบว่าถูกส่งไปหา Virtual Ethernet Link ของ Calico นั่นเองซึ่ง IP ที่เห็นเหล่านี้ก็จะเป็น IP ของ Pod ไม่ก็ Service ต่างๆที่อยู่ใน Kubernetes หมดเลยซึ่งเพื่อนสามารถลองไป Mapping ดูได้จะได้ Link Interface IP ตรงกันเป๊ะๆ โดยให้ดูที่ Pod IP `172.17.168.54` และมี Interface เป็น `linkcalicaadabe44f2` แต่เวลาเราค้นหาผ่าน Interface ใน `ip addr` เราจะใส่แค่ `calicaadabe44f2`

![redhat-advance-cluster-security](images/net/node-net.png)

เข้ามาดู Pod IP `172.17.168.54` จะอยู่บน Node `10.244.0.28`
![redhat-advance-cluster-security](images/net/real-pod.png)

ค้นหา Interface `calicaadabe44f2` จะพบว่า Match ชื่อตรงกันจริงๆ
![redhat-advance-cluster-security](images/net/link.png)

https://projectcalico.docs.tigera.io/about/about-ebpf
โดยภาพแบบประกอบรวมกันแบบสมบูรณ์แล้วก็จะออกมาเป็นดั่งนี้ซึ่งตัวอย่างภาพต้องขอบคุณ Calico CNI ซึ่งหลักการก็คือในส่วนของการ Routing จาก Virtual Ethernet Cable นั้นจะไม่ไปเรียก Iptables เหมือนปกติเพื่อทำการ Routing แต่จำใช้คำสั่ง tc (Traffic Control) ในแต่ล่ะ Calico Network Interface 

![redhat-advance-cluster-security](images/ebpf/cni-ebpf.png)

เพื่อไปดูว่ากำลังมี Packet ไหน Routing มาโดยดูจาก Conntrack (เป็นหนึ่งใน Life Cycle ของการ Routing Packet) ซึ่งดูได้จาก https://en.wikipedia.org/wiki/Netfilter (ต้องขอบอกว่าผมเองก็ยังไม่เคยไปเขียนคำสั่งอะไรยุ่งกับระดับ Low Level ขนาดนี้ตรงๆนะคับ TwT เห็นแล้วน้ำตาไหล 5555) และพอมันไม่ผ่าน Iptables ก็เลยไม่เจอปัญหาการ Lookup จาก Iptables ที่ช้านั่นเอง
![redhat-advance-cluster-security](images/ebpf/flow.png)

ซึ่งที่พูดมาถึงตอนนี้เพื่อนๆอาจจะฟังดูแล้วมันเกี่ยวกับอะรั้ยกับ Advance Cluster Security วันนี้ 5555 วันนี้จะได้เห็น Scan Log4Shell มั้ยยยย 55555 ก็ขอบอกว่าอีกแปปหนึ่งใกล้จบบทแล้ว เพียงแต่ว่าอยากลองเล่าประสบการณ์ที่เกิดจากการอ่านและทดลองทำแบบรวมๆหลายแหล่งมาให้เพื่อเป็นไอเดียพื้นฐานให้เห็นว่ามันตรวจจับได้อย่างไรกันแน่ ซึ่งตัวโปรแกรม eBPF ที่สามารถ Custom เหล่านี้เองได้จะเป็นตัวที่ใช้โปรแกรม eBPF ในการดักจับ Event ต่างๆที่เกิดขึ้นจากการเข้าไป Tracing การใช้งานต่างๆผ่าน Kernel  ซึ่งถ้าเราลองใช้คำสั่งแบบ `dmesg -H` เราก็จะเห็น Logs บางอย่างที่เกิดขึ้นใน Kernel ได้ (คำสั่งนี้เป็นการยกตัวอย่างให้เห็นไอเดียเฉยๆซึ่ง ebpf โปรแกรมเองก็จะมีคำสั่งแบบนี้แต่ใช้ในการ tracing metrics โดยเฉพาะเช่นกันสามารถไปดูที่ repo github https://github.com/iovisor/bpftrace) โดยจากภาพเราจะเห็น link Interface ของ Calico ที่อาจจะเพิ่มหรือลบอยู่ตลอดเวลาตาม Pod ที่ถูก Schedule ไปยัง Host นั่นเอง 

![redhat-advance-cluster-security](images/net/trace.png)

และ Microsoft เองยังคิดจะมีแผนเอามาใช้ใน Window ด้วยนะเป็นอะไรที่ข้ามจักรวาลและเปิดโลกใหม่จริงๆ ซึ่งก็คงเกิดขึ้นได้เพราะอย่าง Window Subsystem Linux 2 เกิดขึ้นก็ช่วยให้ Developer ทำงานสะดวกมากขึ้นจริงๆ (แต่ eBPF Module ก็อาจจะไม่ได้มาครบสมบูรณ์แบบ Linux แต่น่าจะมาบาง Module ที่สำคัญๆ)
https://cloudblogs.microsoft.com/opensource/2021/05/10/making-ebpf-work-on-windows/
![redhat-advance-cluster-security](images/net/window.png)


### Architecture

หลังจากที่เราได้เห็นภาพรวมของจักรวาล eBPF ที่ผมเองก็รู้สึกอึ้งเหมือนกันไปแล้ว 55555 (รู้สึกตัวเองอยู่บนผิวยอดเขา Everest ข้างล่างมีทะเลลึกมากซ่อนอยู่ข้างใต้ TwT) เรากลับมาดูสิ่งที่เกี่ยวข้องกับการลงกันครั้งนี้กัน โดยสำหรับ Component จะประกอบไปด้วยกัน 5 ตัวดั่งตารางนี้
1. Central เป็น User Interface ให้เราสร้างกฏต่างๆว่าจะมีเงื่อนไขอย่างไรได้บ้างซึ่งการต่อ API กลับมาเพื่อนใช้ CLI อย่าง `roxctl` เพื่อแสกนใน Pipeline ก็จะต้องต่อผ่าน Endpoint ของ Central นี้
2. Scanner ใช้ CoreOS Clair ในการแสกน Container Image ซึ่งจะมี Database ที่ใช้ในการเก้บรายการของ vulnerabilities ต่างๆแต่ถ้าอยากได้การอัพเดทช่องโหว่ใหม่ๆก็ต้องเชื่อมต่อ Internet นะ
3. Sensor จะอยู่ใน Cluster ที่เราต้องการจะ Secured ทำหน้าที่รับคำสั่งกฏต่างๆมาจาก Central และ Cluster นี้ก็จะคอยอัพเดท Policy อยู่ตลอดเวลา
4. Admission Controller ใช้ในการตรวจเงื่อนไขตาม Policy ที่ถูกสั่งมาจาก Central ว่า Pod จะถูกสร้างได้สำเร็จหรือไม่
5. Collector จะอยู่ใน Cluster ที่เราต้องการจะ Secured ทำหน้าที่เก็บข้อมูลต่างๆจาก Host โดยตรงว่าตอนนี้ Host ปัจจุบันมีการเรียก Syscall ที่เกี่ยวข้องกับ Network หรือ Process ใดบ้างเพื่อสร้างเป็นแผนภาพ Topology Network นั่นเองโดยจะใช้ eBPF Tracing ไปดึงข้อมูลมาและส่งข้อมูลกลับไปที่ Sensor

![redhat-advance-cluster-security](images/intro/arch.png)


### Install Red Hat Advanced Cluster Security
ในตัวอย่างการ Demo ครั้งนี้จะใช้ Openshift 4.8 ในการลงทั้ง Central ที่เป็น GUI ในการควบคุมกฏต่างๆรวมถึงให้เป็น Cluster ที่ถูก Enforced การใช้กฏด้วยเช่นกัน ซึ่งวิธีการลงก็ให้เราเปิด Operator Catalog ได้เลยที่ Tabs Administration

![redhat-advance-cluster-security](images/install/catalog.png)

ซึ่งสำหรับ Central เราสามารถเลือกทุกอย่างเป็น Default ได้เลยผ่าน GUI หรือจะเลือก Apply Manifest นี้ manual ก็ได้โดยจะไปติดตั้งที่ Namespace `rhacs-operator` 

#### Install: Central
central.yaml
```yaml
apiVersion: platform.stackrox.io/v1alpha1
kind: Central
metadata:
  name: stackrox-central-services
  namespace: rhacs-operator
spec:
  central:
    exposure:
      loadBalancer:
        enabled: false
        port: 443
      nodePort:
        enabled: false
      route:
        enabled: true
    persistence:
      persistentVolumeClaim:
        claimName: stackrox-db
  egress:
    connectivityPolicy: Online
  scanner:
    analyzer:
      scaling:
        autoScaling: Enabled
        maxReplicas: 5
        minReplicas: 2
        replicas: 3
    scannerComponent: Enabled
```

ให้เราใช้คำสั่งนี้เพื่อสร้าง Central หรือจะกดผ่าน GUI ดั่งภาพข้างล่างก็ได้
```
oc apply -f central.yaml
```

![redhat-advance-cluster-security](images/install/installed.png)

ซึ่งจริงๆแล้วเพื่อนๆก็สามารถปรับได้ตามใจชอบเลยนะไป Namespace อื่นก็ได้แต่ SecuredCluster ต้องอยู่ Namespace เดียวกับ Central นะเพราะว่าเราใช้ Default Endpoint ไมไ่ด้ Custom อะไรเพิ่มเติม แต่ถ้ากรณีมีหลายๆ Cluster ก็ให้ไปเพิ่ม Field ระบุเป็น Endpoint จริงๆของ Central ไปนั่นเอง

ไม่ต้องใส่อะไรเพิ่มเติมสามารถใช้ Default ค่าได้เลยสำหรับ Central
![redhat-advance-cluster-security](images/install/central.png)

หลังจากที่เรามี Central แล้วสิ่งที่สำคัญมากก่อนจะไปลง SecuredCluster เพื่อให้เกิดการ Conenct ระหว่าง Central และ Secured Cluster นั่นก็คือเราต้องมี Credentials เพื่อให้ SecuredCluster สามารถต่อกลับไปหา Central ได้ซึ่งเราจะต้องไป Generate ผ่าน GUI  หรือ Command Line ก็ได้ ซึ่งถ้าอยากทำแบบ Command Line เพื่อนๆสามารถไปดูได้ที่ https://docs.openshift.com/acs/3.66/installing/install-ocp-operator.html#roxctl-generate-init-bundle_install-ocp-operator แต่เพื่อให้ทุกคนเห็นภาพแบบเข้าใจง่ายไปพร้อมๆกันเราจะลองทำจากแบบ GUI กันก่อน

ซึ่งเราจะต้องเข้าไปที่ GUI Console ของ Central หลังจากติดตั้งเสร็จเรียบร้อยก็ให้เราที่ Route และเข้าไปยัง Endpoint ของ Central (เลือกตัวชื่อ central เฉยๆ)
![redhat-advance-cluster-security](images/install/central-route.png)

ถ้าเปิดถูกต้องแล้วจะได้หน้าเว็บนี้ขึ้นมา
![redhat-advance-cluster-security](images/install/central-web.png)

ซึ่ง Central จะมี Username default คือ `admin` และ password คือ Secret ที่เราต้องนำมาจาก Namespace ที่ติดตั้ง Central โดยให้เราไปที่ Secret ของ Namespace ที่ลง Central และเราจะพบว่ารหัสผ่านถูก genrate ด้วย Apache `htpasswd` ก็ให้เราเข้าไปที่ Secret ตัวนั้น

![redhat-advance-cluster-security](images/install/ls-secret.png)

copy Password จริงๆมาใช้งานซึ่งรหัสผ่านจริงๆต้องนำมาจาก key `password` นะไม่ใช่ `htpasswd` เพราะตัวนั้นคือ password ที่ผ่านการ Hash แล้วจึงไม่ใช่ password ที่ใช้เข้า (เหมือนที่เราใช้คำสั่ง `htpasswd` ในการสร้างไฟล์ secret)

![redhat-advance-cluster-security](images/install/secret.png)

ซึ่งขั้นตอนตรงนี้สำคัญมากๆเพราะว่าถ้าเราไปลง SecuredCluster ก่อนโดยยังไม่เคยเข้าไป Generate Secret สำหรับเชื่อมต่อกับ Central แล้วไปลง Secured Cluster เลยเราจะเจอ Error ดั่งภาพ

![redhat-advance-cluster-security](images/install/pitfall.png)

แต่ถ้าเพื่อนๆสามารถเข้าไปหน้า Console ได้แล้วก็สบายใจได้ซึ่งระหว่างนี้ทุกคนจะยังไม่มีผลลัพธ์ของการ Scan Cluster ก็ไม่ต้องตกใจไป ~ (เพราะผมแอบลงไปก่อนแล้วนั่นเอง 55555) ซึ่งให้เพื่อนทุกคนโหลด CLI Binary ลงมาก่อนด้วยเลยก็ได้ไว้เตรียมใช้ต่อไป

![redhat-advance-cluster-security](images/integrated/welcome.png)

จากนั้นให้เราไปที่ Tab `Platform Configuration > Integrations` ซึ่ง Tab อยู่ทางด้านซ้ายล่างสุดและกด expand ออกมาดั่งภาพ

![redhat-advance-cluster-security](images/integrated/integrate-cluster.png)

จากนั้นเราจะทำการตั้งชื่อ Secret Clsuter กันสักหน่อยซึ่งก็ให้เป็นชื่อ Cluster ที่เพื่อนๆใช้กันก็ได้นะเพื่อความจำง่ายโดยกดที่ปุ่ม `Generate Bundle` ปุ่มน้ำเงินๆเข้ม

![redhat-advance-cluster-security](images/integrated/generate.png)

เข้าไปกรอกรายละเอียด Cluster ต่างๆและ Generate มาเป็น Secret File ธรรมดาๆสำหรับการทดลองใช้ง่ายๆครั้งนี้ และเราจะได้ไฟล์ Secret มาก็ให้เรานำไป Apply ใช้
![redhat-advance-cluster-security](images/integrated/download.png)

แต่สิ่งสำคัญหนึ่งก็คือ File Secret นี้ต้องอยู่ใน Namespace เดียวกับ SecuredCluster ด้วยนั่นเอง (เพราะไม่อย่างนั้นจะเจอปัญหา SecuredCluster หา Secret ต่อไป Central ไมไ่ด้เหมือนภาพปัญหาตอนที่ลืม Generate Secret ในตอนแรกก่อนนั่นเอง)
ดั่งนั้นเพื่อเช็คกันพลาดสักหน่อยก็ลองตรวจดู Namespace ดูด้วยนะ

ตัวอย่างหน้าตาไฟล์ secret ชื่อ `clsuter-init-secrets.yaml`
```yaml
# This is a StackRox cluster init bundle.
# This bundle can be used for setting up any number of StackRox secured clusters.
# NOTE: This file contains secret data and needs to be handled and stored accordingly.
#
#   name:      "accounting-erp-cluster"
#   createdAt: 2022-01-08T06:12:55.742039919Z
#   expiresAt: 2023-01-08T06:13:00Z
#   id:        96cc46a6-e943-4a40-961c-f7c39fa26b64
#

apiVersion: v1
kind: Secret
metadata:
  annotations:
    init-bundle.stackrox.io/created-at: "2022-01-08T06:12:55.742039919Z"
    init-bundle.stackrox.io/expires-at: "2023-01-08T06:13:00Z"
    init-bundle.stackrox.io/id: 96cc46a6-e943-4a40-961c-f7c39fa26b64
    init-bundle.stackrox.io/name: accounting-erp-cluster
  creationTimestamp: null
  name: collector-tls
stringData:
  ca.pem: |
    -----BEGIN CERTIFICATE-----
    mockkkkkkkkkkk
    -----END CERTIFICATE-----
  collector-cert.pem: |
    -----BEGIN CERTIFICATE-----
    mockkkkkkkkkkk
    -----END CERTIFICATE-----
  collector-key.pem: |
    -----BEGIN EC PRIVATE KEY-----
    mockkkkkkkkkkk
    -----END EC PRIVATE KEY-----
---
apiVersion: v1
kind: Secret
metadata:
  annotations:
    init-bundle.stackrox.io/created-at: "2022-01-08T06:12:55.742039919Z"
    init-bundle.stackrox.io/expires-at: "2023-01-08T06:13:00Z"
    init-bundle.stackrox.io/id: 96cc46a6-e943-4a40-961c-f7c39fa26b64
    init-bundle.stackrox.io/name: accounting-erp-cluster
  creationTimestamp: null
  name: sensor-tls
stringData:
  ca.pem: |
    -----BEGIN CERTIFICATE-----
    mockkkkkkkkkkk
    -----END CERTIFICATE-----
  sensor-cert.pem: |
    -----BEGIN CERTIFICATE-----
    mockkkkkkkkkkk
    -----END CERTIFICATE-----
  sensor-key.pem: |
    -----BEGIN EC PRIVATE KEY-----
    mockkkkkkkkkkk
    -----END EC PRIVATE KEY-----
---
apiVersion: v1
kind: Secret
metadata:
  annotations:
    init-bundle.stackrox.io/created-at: "2022-01-08T06:12:55.742039919Z"
    init-bundle.stackrox.io/expires-at: "2023-01-08T06:13:00Z"
    init-bundle.stackrox.io/id: 96cc46a6-e943-4a40-961c-f7c39fa26b64
    init-bundle.stackrox.io/name: accounting-erp-cluster
  creationTimestamp: null
  name: admission-control-tls
stringData:
  admission-control-cert.pem: |
    -----BEGIN CERTIFICATE-----
    mockkkkkkkkkkk
    -----END CERTIFICATE-----
  admission-control-key.pem: |
    -----BEGIN EC PRIVATE KEY-----
    mockkkkkkkkkkk
    -----END EC PRIVATE KEY-----
  ca.pem: |
    -----BEGIN CERTIFICATE-----
    mockkkkkkkkkkk
    -----END CERTIFICATE-----
```
ใช้คำสั่งและ apply ไฟล์ที่เราเป้นคน generate มาเอง

```
oc project

oc apply -f <ชื่อไฟล์ secret ที่ generate ออกมา>
```
ถ้าเรามี  Secret ครบทั้งสามตัวคือ `sensor-tls` `collector-tls` `admission-control-tls` พร้อมแล้วทีนี้ Component SecuredCluster ก็จะพร้อมใช้งานแล้วนั่นเอง ~


#### Install: SecuredCluster
ให้เรากลับไปที่ Console Openshift อีกครั้งและเตรียมไปสร้าง Kind `SecuredCluster` โดย ให้มั่นใจว่าติ้กเลือก Fields ในภาพให้ครบเพื่อให้ In-Line Scan ทำงานทันทีเวลามี Image ใหม่เข้ามาส่วนที่เหลือสามารถใช้ Default ได้เลยไม่ต้องกดอะไรเพิ่มเติม
![redhat-advance-cluster-security](images/install/securedcluster.png)

โดยตัวอย่างไฟล์แบบที่ต้องการและใช้เป็น Default มีการตั้งค่าตาม GUI จะได้เป็นดั่งนี้ซึ่งก็สามารถสร้างตามนี้ดว้ยการ apply manifest
`oc apply -f securedcluster.yaml` เช่นกัน

securedcluster.yaml
```yaml
apiVersion: platform.stackrox.io/v1alpha1
kind: SecuredCluster
metadata:
  name: stackrox-secured-cluster-services
  namespace: rhacs-operator
spec:
  admissionControl:
    bypass: BreakGlassAnnotation
    contactImageScanners: ScanIfMissing
    listenOnCreates: true
    listenOnEvents: true
    listenOnUpdates: true
    timeoutSeconds: 3
  auditLogs:
    collection: Auto
  clusterName: thai-cluster
  perNode:
    collector:
      collection: KernelModule
      imageFlavor: Regular
    taintToleration: TolerateTaints
```

ถ้าหากลงได้สมบูรณ์แล้วจะได้ภาพดั่งนี้คือมี `Central` และ `SecuredCluster` ที่ Deploy เรียบร้อยแล้ว

![redhat-advance-cluster-security](images/integrated/final.png)

ให้เรากลับไปที่ Console ของ Advanced Cluster Security จะเห็น Cluster ที่เพิ่มเข้าไปสำเร็จเพิ่มขึ้นมาซึ่งชื่อ Cluster ก็จะเป็นชื่อของ Manifest ที่เราสร้างนั่นเองซึ่งการ Config ถ้าอยากจะแก้ไขต้องไปแก้ไขผ่าน Manifest ของ Kind `SecuredCluster` 

![redhat-advance-cluster-security](images/integrated/connected.png)

ภาพของ SecuredCluster ที่แจ้งเตือนว่าถูกสร้างจาก Custom Resource Definition ของ Operator

![redhat-advance-cluster-security](images/integrated/securedclsuter-detail.png)

ถึงตอนนี้ก็เรียกว่าครบภาพรวมการติดตั้ง Advanced Cluster Security แบบง่ายๆไปเรียบร้อยแล้วนั่นเองโดยให้เพื่อนๆกลับไปที่ tab Compliance แล้วลองกด Scan Environment สักรอบหนึ่งถ้าถูกต้องจะต้องสามารถ Scan และได้ Report ออกมาเหมือนกับภาพนี้

![redhat-advance-cluster-security](images/integrated/compliance.png)





```
roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-7q7o5f-4b4a324f027aea19c5cbc0c3275c4656-0000.hkg02.containers.appdomain.cloud:443 --image quay.io/linxianer12/java-danger-log4j:0.0.4  --token-file token
```


https://docs.openshift.com/acs/3.66/installing/install-ocp-operator.html

On the RHACS portal, navigate to Platform Configuration → Integrations.

Under the Authentication Tokens section, click on Cluster Init Bundle.

Click New Integration.

Enter a name for the cluster init bundle and click Generate.

Click Download Kubernetes Secret File to download the generated bundle.


```
oc run shell --labels=app=shellshock,team=test-team --image=quay.io/linxianer12/vulnerables:cve-2014-6271 

oc run samba --labels=app=rce  --image=docker.io/vulnerables/cve-2017-7494 

roxctl deployment check --file deplyoment.yaml --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443  --token-file token

roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443 --image quay.io/linxianer12/java-danger-log4j:0.0.1  -f  --token-file token

roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443 --image quay.io/linxianer12/im-in-frontend:0.0.3  -f  --token-file token 

roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443 --image quay.io/linxianer12/vulnerables:cve-2014-6271  -f  --token-file token 

#  have >=High Severity Vulnerable but Fixable in next release already

oc create deployment java-danger  --image quay.io/linxianer12/java-danger-log4j:0.0.1 

# have >=High Severity Vulnerable but Fixable in next release already
oc create deployment im-in-frontend --image quay.io/linxianer12/im-in-frontend:0.0.3

# have >=High Severity Vulnerable but Fixable in next release already
oc create deployment todoapp-frontend --image quay.io/linxianer12/todoapp-frontend:1.0.0

# Not have Fixable yet
oc create deployment shellshock --image quay.io/linxianer12/vulnerables:cve-2014-6271

```