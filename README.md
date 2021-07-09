# FAQ
Вопросы на которые все устали отвечать

- [FAQ](#faq)
    - [**Как перехватить трафик приложения?**](#как-перехватить-трафик-приложения)
    - [**Пытаюсь перехватить трафик так, как написано выше, ничего не выходит, что делать?**](#пытаюсь-перехватить-трафик-так-как-написано-выше-ничего-не-выходит-что-делать)
    - [**Как получить права root на смартфоне?**](#как-получить-права-root-на-смартфоне)
    - [**Как написать вирус?**](#как-написать-вирус)
    - [**Как шифровать данные?**](#как-шифровать-данные)
    - [**Как сделать проверку на root?**](#как-сделать-проверку-на-root)
    - [**Не могу установить/заставить работать/скачать %название инструмента%, что делать?**](#не-могу-установитьзаставить-работатьскачать-название-инструмента-что-делать)
    - [**Чем декомпилировать код приложения?**](#чем-декомпилировать-код-приложения)
    - [**Как изменить поведение чужого приложения?**](#как-изменить-поведение-чужого-приложения)
    - [**Как подписать пересобранное приложение?**](#как-подписать-пересобранное-приложение)
    - [**Можно ли изменить приложение с сохранением оригинальной цифровой подписи?**](#можно-ли-изменить-приложение-с-сохранением-оригинальной-цифровой-подписи)
    - [**Код приложения обфусцирован, всюду китайские символы и зашифрованные строки. Что делать?**](#код-приложения-обфусцирован-всюду-китайские-символы-и-зашифрованные-строки-что-делать)
    - [**Как получить apk файл приложения?**](#как-получить-apk-файл-приложения)
    - [**Где почитать про уязвимости в приложениях и техники их устранения**](#где-почитать-про-уязвимости-в-приложениях-и-техники-их-устранения)

### **Как перехватить трафик приложения?**

Взять [Charles](https://www.charlesproxy.com/), [Fiddler](https://www.telerik.com/fiddler), [Burp Suite](https://portswigger.net/burp) или [mitmproxy](https://mitmproxy.org/) и нагуглить любой понравившийся мануал. Секретный google дорк может выглядеть так: *charles proxy intercept android traffic.*

Общий алгоритм в любом мануале как правило сводится к такому:

1. Запустить прокси на компьютере
2. Установить на устройство сертификат предоставляемый проксей
3. В настройках сети на смартфоне указать адрес хостовой машины и порт
4. Запустить на смартфоне приложение, которое нужно изучить
5. ???????
6. PROFIT!

В отдельных, клинических случаях может потребоваться использование приложения вроде [ProxyDroid](https://github.com/madeye/proxydroid) (нужен [root](#как-получить-права-root-на-смартфоне))

### **Пытаюсь перехватить трафик так, как написано выше, ничего не выходит, что делать?**

Скорее всего в приложение применяется ssl pinning. Иногда он отключается просто, иногда не очень. Простые способы:

- В [APKLab](https://github.com/APKLab/APKLab) есть опция пересборки приложения с отключением пиннинга
- Похожая опция есть в [Objection](https://github.com/sensepost/objection)
- В Xposed под это есть модули: [SSLUnpinning](https://repo.xposed.info/module/mobi.acpm.sslunpinning) и [JustTrustMe](https://github.com/Fuzion24/JustTrustMe)
- [Скрипт](https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/) для Frida

Если ничего из приведенного выше не помогло победить пиннинг, то выкорчевывать его придется руками. Пример такой работы есть в [этом](https://www.youtube.com/watch?v=Xn6CSqJpf6I) видео.

### **Как получить права root на смартфоне?**

Самый простой и актуальный способ — использовать [Magisk](https://magisk.me/), если он не работает, то искать конкретное решение для вашей модели или смириться.

### **Как написать вирус?**

Никак, это запрещено [ст. 273 УК РФ](https://www.consultant.ru/document/cons_doc_LAW_10699/a4d58c1af8677d94b4fc8987c71b131f10476a76/)

### **Как шифровать данные?**
<!-- markdown-link-check-disable -->
Если нужно шифровать просто что-нибудь, то лучше использовать библиотеку [Tink](https://github.com/google/tink). Но как правило нужно шифровать преференсы или файлы. В этом случае нужно использовать библиотеку [security-crypto](https://developer.android.com/jetpack/androidx/releases/security) из Android JetPack
<!-- markdown-link-check-enable -->
### **Как сделать проверку на root?**
<!-- markdown-link-check-disable -->
Использовать библиотеку [RootBeer](https://github.com/scottyab/rootbeer) или [SafetyNet Attestation API](https://developer.android.com/training/safetynet/attestation)
<!-- markdown-link-check-enable -->
### **Не могу установить/заставить работать/скачать %название инструмента%, что делать?**

Если не получается что-то сделать по официальной инструкции, то тут возможно два варианта:

1. Нарушены системные требования (версии библиотек и т.п.)
2. Вы используете не ту версию, которая описана в инструкции

Вторая проблема решается просто — всегда сверяйтесь с версиями утилит для которых написан мануал.

С первой проблемой все несколько сложнее, потому что системные требования не всегда очевидны. В большинстве случаев от этого спасает Docker, т.к. позволяет не гадить в системе и легко создавать нужные окружения.

### **Чем декомпилировать код приложения?**

Если денег нет, то [Jadx](https://github.com/skylot/jadx), если деньги есть то [Jeb](https://www.pnfsoftware.com/jeb/android)

### **Как изменить поведение чужого приложения?**

Распаковать с помощью [apktool](https://ibotpeaches.github.io/Apktool/), внести правки в smali-код в любом текстовом редакторе, собрать с помощью apktool обратно и подписать своим ключом. 

### **Как подписать пересобранное приложение?**

```bash
# Сгенерировать ключ
$ keytool -genkey -v -keystore fake.keystore -storepass fake-fake -alias fake -keypass fake-fake -keyalg RSA -keysize 2048 -validity 10000

# "Выровнять" и подписать приложение
$ zipalign -c 4 app-release/dist/app-release.apk
$ apksigner sign --ks fake.keystore --ks-key-alias fake --ks-pass pass:fake-fake --key-pass pass:fake-fake app-release/dist/app-release.apk
```

### **Можно ли изменить приложение с сохранением оригинальной цифровой подписи?**

Нет

### **Код приложения обфусцирован, всюду китайские символы и зашифрованные строки. Что делать?**

Простые случаи обфускации вполне себе разматывает [Jadx](https://github.com/skylot/jadx), для более сложных можно применить [Simplify](https://github.com/CalebFenton/simplify) или [DeGuard](http://apk-deguard.com/). Но иногда вообще ничего не помогает и придется тратить часы/дни/годы на распутывание поведения приложения.

### **Как получить apk файл приложения?**
<!-- markdown-link-check-disable-next-line -->
Взять с сайта [APKPure](https://apkpure.com/)/[APKMirror](https://www.apkmirror.com/) или [стянуть](https://github.com/Android-Guards/apk-extractor) уже установленное с устройства. 

### **Где почитать про уязвимости в приложениях и техники их устранения**

- [OWASP Mobile Top Ten](https://owasp.org/www-project-mobile-top-10/)
- [OWASP Mobile Application Security Verification Standard](https://github.com/OWASP/owasp-masvs)
- [Security Code Smells in Android ICC](https://arxiv.org/pdf/1811.12713.pdf)
- [android-app-vulnerability-benchmarks](https://bitbucket.org/secure-it-i/android-app-vulnerability-benchmarks/src/master/)
- [Oversured(blog)](https://blog.oversecured.com/)