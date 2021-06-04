# เก็บรหัสผ่านเป็น Plaintext

_เรื่องนี้ขอจัดอยู่ในหมวด Rumor เพราะไม่มีหลักฐานว่าตัวระบบที่อ.กบทำขายจริง ๆ แล้วได้ทำการเก็บรหัสผ่านเป็น Plaintext จริงหรือไม่ มีข้อมูลเพียงแค่เรื่องเล่าดังกล่าวนี้เท่านั้น โดยเป็นข้อมูลที่เป็นการสันนิษฐานจากข้อมูลที่รวบรวมมาเท่านั้น ไม่เป็นการยืนยันหรือกล่าวหาแต่อย่างใด_

![](https://user-images.githubusercontent.com/78331701/106409627-8a586e80-6473-11eb-96f2-982d93382eed.png)

ช่วงเช้าของวันที่ 1 กุมภาพันธ์ 2564 อ.กบได้ทำการโพสสิ่งที่ดูเหมือนจะเป็นเรื่องเล่าตลก ๆ ลงกลุ่มนักเขียนโปรแกรม ซึ่งถ้าอ่านผ่าน ๆ ก็จะดูเหมือนเรื่องเล่าตลกขำขันคล้าย ๆ [Tales From Tech Support](https://www.reddit.com/r/talesfromtechsupport/) เวอร์ชั่นภาษาไทย

แต่หากดูดี ๆ แล้วมีอยู่จุดหนี่งที่กล่าวไว้ว่า:

```
เอางี้ครับผมขอรหัสที่ใช้ครั้งสุดท้าย
...
ทำการค้นในระบบ เพื่อรีเซตรหัสให้ลูกค้าคนนี้
```

สามารถตีความได้ว่าเป็นขอรหัสผ่านของลูกค้า เพื่อเข้าไปค้นหาภายในระบบ ซึ่งหากใครที่ทำงานหรือมีความรู้เกี่ยวกับเรื่อง Security หรือ Authentication น่าจะทราบดีว่า ไม่สามารถนำรหัสผ่านที่เป็น Plaintext เข้าไปค้นหาในฐานข้อมูลได้ ([ทำไม?](#ทำไมถึงเอาไปหาในฐานข้อมูลไม่ได้))

แต่ข้อมูลทั้งหมดที่ทราบก็มีเพียงเท่านี้ ไม่มีหลักฐานอื่นยืนยันว่าระบบของอ.กบได้เก็บรหัสผ่านเป็น Plaintext จริง ๆ แต่ถึงแม้จะเป็นเรื่องแต่งก็จะเป็นเรื่องแต่งที่เผยแพร่ Bad Practice ในการพัฒนาระบบอยู่ดี

> การเก็บเป็น Plaintext คือเก็บข้อมูลแบบไม่ได้เข้ารหัสเลย เช่น หากผู้ใช้ตั้งรหัสผ่านว่า `password` ข้อมูลในฐานข้อมูลก็จะเก็บว่า `password` ไปเลย ไม่ได้มีการแปลงหรือเข้ารหัสข้อมูลแต่อย่างใด

---

## ทำไมถึงเอาไปหาในฐานข้อมูลไม่ได้?

การเก็บรหัสผ่านลงฐานข้อมูล แม้ว่าตัวฐานข้อมูลจะมีการป้องกันที่ดีเพียงใด ข้อมูลรห้สผ่านก็ควรจะเข้ารหัสอยู่เสมอ เพื่อป้องกันที่ว่าหากฐานข้อมูลโดนเจาะเข้าไป ผู้ประสงค์ร้ายก็จะไม่ได้ข้อมูลที่เป็นความลับนั้นไป

- Q: ถ้าผู้ประสงค์ร้ายได้รหัสผ่าน (ที่เข้ารหัสแล้ว) ไป เขาก็เอาไปถอดรหัสสิ มันจะปลอดภัยขึ้นได้ไง?
  - A: การเข้ารหัสส่วนใหญ่จะเป็นการ Hashing ซึ่งเป็น one-way function ที่เมื่อเข้ารหัสแล้ว จะถอดรหัสไม่ได้ (จริง ๆ ถอดได้แต่ทำได้ยาก)
- Q: แล้วทำไมถึงเอาไปหาในฐานข้อมูลไม่ได้?
  - A: จากคำถามข้อที่แล้ว เราไม่สามารถทราบได้ว่ารหัสผ่านที่เก็บไว้จริง ๆ คืออะไร เพราะถูกเข้ารหัสไว้ และไม่สามารถถอดรหัสได้
  - แต่ในกรณีของอ.กบที่ใช้ [kob256](#เพิ่มเติม-2) (ฟังก์ชันการเข้ารหัสสมมุติ) สามารถนำรหัสผ่านที่เข้ารหัสด้วยวิธีที่เป็นความลับของบริษัทแล้ว ไปแปลงกลับเป็นรหัสผ่านที่เป็น Plaintext ได้

---

## เพิ่มเติม 1

มีผู้ที่บอกว่าเคยใช้ระบบ jPOS (ระบบ POS อันดับ 1 ของไทย) ของอ.กบ บอกว่าตัวโปรแกรมแบบออฟไลน์สามารถเข้าถึงฐานข้อมูลได้ ซึ่งก็เห็นว่ารหัสผ่านของตัวระบบนี้เก็บเป็น Plaintext

![](https://user-images.githubusercontent.com/78331701/106413763-7e71aa00-647d-11eb-84aa-3b9a8477639b.jpg)

---

## เพิ่มเติม 2

ต่อมาได้มีสมาชิกสงสัยว่าทำไมถึงสามารถเอารหัสผ่านเข้าไปค้นหาในระบบได้ ซึ่งคำตอบที่ได้ก็คือ

> ความลับบริษัท

![q2](https://user-images.githubusercontent.com/78331701/106413285-4a49b980-647c-11eb-8c53-59a785806026.jpg)
![q1](https://user-images.githubusercontent.com/78331701/106413288-4cac1380-647c-11eb-8342-015e58eef82a.jpg)

ข้อมูลส่วนนี้ก็ได้มาเพิ่มเติมว่าอ.กบอาจไม่ได้เก็บรหัสผ่านเป็น Plaintext จริง ๆ แต่ว่าเข้ารหัสแบบที่นำไปค้นหาได้ด้วยวิธีการที่เป็นความลับของบริษัท

*ต่อไปนี้ขอเรียกฟังก์ชันการเข้ารหัสที่เป็นความลับนี้ว่า `kob256` (ชื่อสมมุติอย่างไม่เป็นทางการ) เพื่อความง่ายในการกล่าวถึง*

---

## เพิ่มเติม 3

_(ไม่เกี่ยวข้องกับเนื้อหาหลัก)_

ภายหลังได้มีผู้มาแสดงความคิดเห็นว่าโพสนี้ผิดกฏกลุ่ม และขอให้แอดมินกลุ่ม (ซึ่งก็คืออ.กบ) เตะเจ้าของมุขนี้ออกจากกลุ่ม

![](https://user-images.githubusercontent.com/78331701/106412153-95ae9880-6479-11eb-94ad-10a5ccecf4ed.jpg)

ซึ่งอ.กบก็ทำตามที่เรียกร้องแต่เป็นการใช้ Uno Reverse Card ทำการเตะผู้ที่เรียกร้องออกจากกลุ่มแทน

![](https://user-images.githubusercontent.com/78331701/106423876-d74c3d00-6493-11eb-8627-a79a3fd68132.jpg)

---

## สรุป

จากข้อมูลทั้งหมดก็ยังไม่สามารถบอกได้ว่าอ.กบได้เก็บข้อมูลรหัสผ่านเป็นอย่างไรกันแน่ แต่ก็ได้ข้อสันนิษฐานดังนี้คือ:

- เก็บเป็น Plaintext
  - สามารถนำไปต้นหาในฐานข้อมูลได้ทันที
- เข้ารหัสด้วยการเข้ารหัสทั่วไป เช่น DES, AES
  - ที่อ.กบถือคีย์ที่เป็นความลับไว้ เพื่อใช้ถอดรหัสในกรณีที่ต้องการรีเซ็ตรหัสผ่านให้ลูกค้า
- เข้ารหัสด้วย [kob256](#เพิ่มเติม-2)
  - ที่เป็นฟังก์ชัน Hashing สามารถถอดรหัสกลับมาได้

หมายเหตุ - เนื้อหานี้ไม่ได้มีเจตนาในการโจมตีบุคคลอื่นใด จุดประสงค์หลักคือต้องการให้ผู้คนทราบถึงการจัดเก็บรหัสผ่านด้วยวิธีที่ปลอดภัยและได้รับการยอมรับเท่านั้น