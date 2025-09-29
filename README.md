```javascript
/**
     * Copyright © 2025 [ balxzzy ]
     *
     * All rights reserved. This source code is the property of [ Shiina Team ].
     * Unauthorized copying, distribution, modification, or use of this file,
     * via any medium, is strictly prohibited without prior written permission.
     *
     * This software is protected under international copyright laws.
     *
     * Contact: [ pa424013@gmail.com ]
     * GitHub: https://github.com/balxz
     * Official: https://balxzzy.web.id
     * Support: https://t.me/sh_team1
 */
import axios from "axios"

class bAuth {
    constructor() {
        /** ganti jdi raw repo mu yh **/
        this.dbnyaJing = "https://raw.githubusercontent.com/balxz/db-bot-access/refs/heads/main/auth.json"
        this.aksesNya = []
    }

    async loadN() {
        try {
            let { data } = await axios.get(this.dbnyaJing)
            this.aksesNya = data
            console.log("[ 🪷 ] sedang mengecek nomor di database...")
        } catch (error) {
            console.log("[ 🪷 ] gagal cek nomor, apakah url raw benar?")
        }
    }
    checkN(dongo) {
        let woi = dongo.replace(/[^\d+]/g, "")
        if (woi.startsWith("+")) {
            woi = woi.substring(1)
        }
        let akses = this.aksesNya.includes(woi)
        
        return {
            allowed: akses,
            message: akses ? "HOREEEE NOMOR TERDAFTAR (SELAMAT MENGGUNAKAN)" : "NOMOR TIDAK TERDAFTAR DI DATABASE (SILAHKAN HUBUNGI PEMILIK SC)"
        }
    }
}

export default bAuth
```
---
```javascript
  // whooo? how to call it?
  // ( ESM )
   import bAuth from "./path/file.js"
   const auth = new bAuth()
   auth.loadN()
   do {
       let input = rl.......
       let abcd = await auth.checkN(input)
       if(!abcd.allowed) {
         console.log(abcd.message)
        continue // bukan return dongo
      } else {
        console.log(abcd.message)
        break
      }
  } while (true)
  // lanjutkan di bawah logika request pairing nya 
```
---
---

```javascript
  // whooo? how to call it?
  // ( CJS )
   const bAuth = require("./path/file")
   const auth = new bAuth()
   auth.loadN()
   do {
       let input = rl.......
       let abcd = await auth.checkN(input)
       if(!abcd.allowed) {
         console.log(abcd.message)
        continue // bukan return dongo
      } else {
        console.log(abcd.message)
        break
      }
  } while (true)
  // lanjutkan di bawah logika request pairing nya 
```

## CONTOHNYA 
---
```javascript
if (!conn.authState.creds.registered) {
    // call auth tadi — load number
    console.log(chalk.blue('[ 🪷 ] Loading...'))
    await auth.loadN()
    
    const { registration } = { registration: {} };
    let phoneNumber = "";
    do {
        phoneNumber = await question(
            chalk.blueBright("Masukkan nomor yang valid, dengan Region: 62xxx:\n")
        );
        
        // call auth tadi — check number
        const checkResult = auth.checkN(phoneNumber)
        if (!checkResult.allowed) {
            console.log(chalk.red(` ${checkResult.message}`))
            console.log(chalk.yellow("Silakan masukkan nomor yang terdaftar...\n"))
            continue
        } else {
            console.log(chalk.green(`${checkResult.message}`))
            break
        }
        
    } while (true);
    
    rl.close();
    phoneNumber = phoneNumber.replace(/\D/g, "");
    console.log(chalk.bgWhite(chalk.blue("Tunggu Sebentar...")));
    setTimeout(async () => {
        let code = await conn.requestPairingCode(phoneNumber);
        code = code?.match(/.{1,4}/g)?.join("-") || code;
        console.log(
            chalk.black(chalk.bgGreen(`Your Pairing Code : `)),
            chalk.black(chalk.white(code))
        );
    }, 3000);
}
```
---












