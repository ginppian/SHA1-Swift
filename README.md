SHA1 Swift
====

## Descripción

Si en algun momento necesitamos enviar datos encriptados podemos hacer uso de **SHA1** para esto existen diversas implementaciónes, desde usar librerias en Obj-C hasta
frameworks o pods que ya traen diversas iplementaciones criptograficas.

En nuestro caso optamos por usar *Extension* ya que el proyecto donde lo vamos a utilizar es demasiado grande y buscamos una solución ligera.

Si queremos implementar un framework recomendamos usar: <a href="https://github.com/krzyzanowskim/CryptoSwift#installation">CryptoSwift</a>

## SHA-1

Para convertir un String Alfanumerico en un String SHA1 seguimos la siguiente lógica:

1. Convertir un Objeto String a un Objeto Data (Convert string to Data object)
2. Encriptar a SHA1 con la opción que trae el Objeto Data (Encrypt data using SHA1 function to Data)
3. Convertir el Objeto Data a una String Hexadecimal (Convert data object to hex string)

## Source

```Swift
//
//  ViewController.swift
//  pruebaSha
//
//  Created by Gmo Ginppian on 20/12/17.
//  Copyright © 2017 BUAP. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    let idUsuario = "2228156389"
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        print(idUsuario.sha1)
    }

}

extension String {
    
    var sha1: String {
        guard let data = data(using: .utf8, allowLossyConversion: false) else {
            return ""
        }
        return data.digestSHA1.hexString
    }
    
}

fileprivate extension Data {
    
    var digestSHA1: Data {
        var bytes: [UInt8] = Array(repeating: 0, count: Int(CC_SHA1_DIGEST_LENGTH))
        
        withUnsafeBytes {
            _ = CC_SHA1($0, CC_LONG(count), &bytes)
        }
        
        return Data(bytes: bytes)
    }
    
    var hexString: String {
        return map { String(format: "%02x", UInt8($0)) }.joined()
    }
    
}
```

## Fuente

* <a href="https://stackoverflow.com/questions/25761344/how-to-crypt-string-to-sha1-with-swift">Stackoverflow - Comment Mateusz Szklarek</a>