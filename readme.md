## Application ที่พร้อม Scale ก็ต้องเสริมความมั่นใจด้วย Security ที่ช่วยตรวจสอบการทำงานในทุกจุด
ปัจจุบันการ Deploy Application ในรูปแบบของ Container นั้นเป็นรูปแบบทีไ่ด้รับความนิยมสำหรับ Project ที่ต้องการคุณสมบัติการ Scale ที่ยืดหยุ่นสามารถ Deploy ได้ง่ายไม่ยึดติดกับ Host หรือ Virtual Machine ที่ทำงานอยู่ ซึ่งด้วยความสะดวกของการ Scale Application ในรูปแบบของ Container ก็ทำให้เรามี Application ที่ถูก Scale Out ออกไปหลายๆแอพและไม่จำเป็นต้องอยู่ในเครื่องเดียวกันก็ได้ แต่กระนั้นเองก็อาจจะเป็นการเพิ่ม Attack Surface ได้เช่นเดียวกันเพราะ Application ไมไ่ด้อยู่แค่ที่ Host Machine เครื่องเดียวอีกต่อไป ซึ่งถ้าหาก Application ที่เรา Deploy ไปนั้นมีช่องโหว่ Vulnerability ในหลายๆที่หลายเครื่อง Machine ตามไปด้วย แต่ไม่ใช่หมายความว่าการ Scale Out Application เป็นสิ่งไม่ดีนะ TwT  เพียงแต่เป็นเรื่องความจริงของ Application ที่อยู่ในรูปแบบ Distributed อยู่แล้วนั่นเองคับผม  ซึ่งก่อนส่งท้ายปี 2021 ที่ผ่านมาเราคงจะจำอภิมหากาฬช่องโหว่ Vulnerability อย่าง Log4Shell ที่อยู่ใน Library ของ Log4J ใน Java นั่นเองซึ่งก็กระทบแทบจะเกือบทุก Application ในแพลตฟอร์มใหญ่ๆที่เราใช้งานทั้งหมดซึ่งสำหรับวิธีการแก้ไขที่ดีที่สุดเลยก็คือการที่เรารีบอัพเกรด Library ของเราให้ไปยัง Version ล่าสุดที่ผ่านการแก้ไขช่องโหว่นี้เรียบร้อยแล้วกับคอยตามช่องโหว่อยู่ตลอดเวลา ซึ่งความยิ่งใหญ่นี้ถึงขนาดที่เว็บไซต์อย่าง Docker Hub ที่ใช้สำหรับฝาก Container Image นั้นมีขึ้นแจ้งเตือน Log4Shell ไว้ที่ตรง Tag ของ Image ที่จะ Pull เลยทีเดียว ซึ่งก็เป็นการช่วยให้เราอุ่นใจได้มากขึ้นสำหรับ Image ใหม่ๆที่เราจะ Pull ลงมาแต่ถ้าหากเราใช้ Private Registry ทั่วๆไปที่เราใช้ในการฝาก Container Image เฉยๆหรือว่า Image ของเรานั้นถูกรันทำงานใน Cluster ไปตั้งนานแล้วเราก็คงจะไม่ทราบว่าแล้ว Image ที่กำลังทำงานอยู่นั้นปลอดภัยดีหรือเปล่าจากช่องโหว่ Log4Shell 

ซึ่งวันนี้เองเราจะลองมาทดลองใช้ Product  Red Hat Advance Cluster Security ซึ่งเป็นส่วนของ Project Opensource ชื่อว่า Open Cluster Management ซึ่งตอนนี้กำลังเป็น Sandbox Project ของ Cloud Native Computing Foundation https://open-cluster-management.io 

![support-matrix](images/intro/banner-open-cluster.png)

ซึ่งไว้เดี่ยวครั้งหน้าเราจะลองมาใช้ Project Open Cluster Management ในการควบคุม Deploy Application ไปหลายๆ Cluster พร้อมกับ Rule เงื่อนไขการ Deploy กันคับผม ซึ่งสำหรับวันนี้ส่วนที่เราจะลองมาเดโม่กันคือส่วนของ Advance Cluster Security ที่ลิ้งค์นี้ https://github.com/stolostron/advanced-cluster-security แต่สำหรับ Demo ครั้งนี้เราจะใช้ Operator ที่อยู่ใน Openshift Cluster ในการติดตั้ง Product Red Hat Advance Cluster Security ซึ่งเป็น Version ที่มี Subscription และได้รับการ Support การใช้งานดูแลจาก Red Hat กันว่าถ้าอยากลอง Config ใช้งานแบบง่ายๆสำหรับสร้าง Rule กฏสำหรับการตรวจจับ Log4Shell และช่องโหว่ Security อื่นๆต้องทำอย่างไรบ้าง
โดย Red Hat Advance Cluster Security นั้นสามารถติดตั้งได้ทั้งใน Vanila Kubernetes และ Openshift เองเพียงแต่ว่าสำหรับ Console ที่เป็นเสมือน Control Plane ในการควบคุม Rule และกฏในการ Secure Cluster(Agent ลงที่เครื่องปลายทาง) ปลายทางที่ต้องเราต้องการป้องกันโดยใช้กฏตามสั่งจาก Control Plane ที่เราจะเรียก Component นี้ว่า Central (Control Plane) โดยสามารถไปดูรายละเอียดเพิ่มเติมได้ที่ https://access.redhat.com/node/5822721

![support-matrix](images/intro/support-matrix.png)

โดยเทคโนโลยีเบื้องหลังของเทคนิค


docker push quay.io/linxianer12/java-danger-log4j:0.0.1 

docker run quay.io/linxianer12/java-danger-log4j:0.0.1 


https://access.redhat.com/node/5822721


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