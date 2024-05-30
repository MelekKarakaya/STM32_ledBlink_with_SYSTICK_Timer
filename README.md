# STM32 İLE SYSTICK TİMER KESMESİ KULLANILARAK LED YAKMA 

Bu projede HAL_Delay yerine Systick kesmesi ve sayaç yöntemi kullanıldı, çünkü 🙃 :

 💡HAL_Delay, programın belirli bir süre boyunca duraklamasına neden olur ve bu süre boyunca mikrodenetleyici diğer görevleri yerine getiremez. Özellikle çoklu görev gerektiren uygulamalarda bu yöntem verimli olmayabilir.

 💡Systick kesmesi ve sayaç yöntemi ise programın kesintili olarak zamanlayıcı işlemler yapmasını sağlar ve diğer görevleri engellemez. Bu yöntem, genellikle çoklu görev işlemlerinde veya gerçek zamanlı sistemlerde tercih edilir çünkü mikrodenetleyiciyi sürekli olarak meşgul etmez ve sistem performansını artırabilir.

1. Öncelikle bir tick_count adında bir değişken tanımlanır  ve volatile türündedir çünkü değişkenin derleyici tarafından optimize edilmesini engeller. Özellikle kesme hizmet rutinleri veya çoklu iş parçacığı programlaması gibi durumlarda kullanılır. 
![image](https://github.com/MelekKarakaya/STM32_ledBlink_with_SYSTICK_Timer/assets/78067331/4ba3b096-4908-4fd4-abf0-e7eb7963cf75)

2. HAL_SYSTICK_Config işlevi, Systick zamanlayıcısını yapılandırır. Bu zamanlayıcı, mikrodenetleyici üzerinde zamanlayıcı kesmeleri üretmek için kullanılır. SystemCoreClock değişkeni, sistem saat hızını temsil eder. Bu değer, SystemClock_Config fonksiyonunda yapılandırılır ve genellikle mikrodenetleyicinin çalıştığı saat frekansını gösterir. SystemCoreClock / 1000 ifadesi, Systick zamanlayıcısının her milisaniyede bir kesme üretmesini sağlar. Yani, 1 saniyede 1000 kesme üretilir. Bu kısım istenilen hassasiyette kesme üretmek içindir.
![image](https://github.com/MelekKarakaya/STM32_ledBlink_with_SYSTICK_Timer/assets/78067331/0fff363e-bed5-4de9-9f51-538b98fd5e08)

3. Bu kod bloğu, tick_count değişkeninin 1000'e ulaştığında yani yaklaşık olarak 1 saniye geçtiğinde bir işlem gerçekleştirir. Bu da HAL_Delay(1000) kullanmak gibidir ancak işlemci üzerindeki diğer görevleri bloklamadan görevi gerçekleştirmektedir.
![image](https://github.com/MelekKarakaya/STM32_ledBlink_with_SYSTICK_Timer/assets/78067331/5af640d1-4269-4cd8-b0aa-eb55162f6fa8)

4. Systick kesme vektörü için bir kesme hizmet rutini olan SysTick_Handler fonksiyonunu içerir. Bu fonksiyonun görevi, Systick kesmesi gerçekleştiğinde yapılacak işlemleri içermektir. tick_count değişkeni her Systick kesmesinde bir arttırılır. Bu değişken, 1 saniye (veya belirlenen zaman aralığı) geçtiğinde kullanılacak bir zamanlayıcı olarak işlev görür. HAL_IncTick() ise Sistem zamanlayıcısını günceller.
![image](https://github.com/MelekKarakaya/STM32_ledBlink_with_SYSTICK_Timer/assets/78067331/095febb0-019f-4d80-9388-eee08e922f60)


5. Proje .ioc şeması verilmiştir.
   
![image](https://github.com/MelekKarakaya/STM32_ledBlink_with_SYSTICK_Timer/assets/78067331/d0ba0690-47dc-439f-83c6-4b6504b306b1)

