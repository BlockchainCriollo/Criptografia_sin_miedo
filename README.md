## 📦 Requisitos

Asegurate de tener Python 3.7+ y los siguientes paquetes instalados:

```bash
pip install ecdsa
```

## 📚 Contenidos

### 1. Hash con SHA-256

Aprendemos a transformar cualquier dato en una "huella digital" única usando el algoritmo SHA-256, el mismo que utiliza Bitcoin.

```python
import hashlib

def hash_sha256(text):
    return hashlib.sha256(text.encode()).hexdigest()

mensaje = "Hola blockchain"
hash_resultado = hash_sha256(mensaje)
print("Mensaje:", mensaje)
print("Hash SHA-256:", hash_resultado)
```

---

### 2. Firma digital con ECDSA (curva SECP256k1)

Aprendemos cómo funcionan las firmas digitales con criptografía asimétrica. Este tipo de firma se usa en las transacciones de Bitcoin para verificar que el emisor es quien dice ser.

```python
from ecdsa import SigningKey, SECP256k1

# Crear clave privada y pública
clave_privada = SigningKey.generate(curve=SECP256k1)
clave_publica = clave_privada.verifying_key

# Mostrar las claves
print("Clave privada:", clave_privada.to_string().hex())
print("Clave pública:", clave_publica.to_string().hex())

# Firmar un mensaje
mensaje = "Transferencia de 1 BTC a María"
mensaje_bytes = mensaje.encode('utf-8')
firma = clave_privada.sign(mensaje_bytes)

# Verificar firma
es_valida = clave_publica.verify(firma, mensaje_bytes)
print("Mensaje:", mensaje)
print("Firma válida:", es_valida)
```

---

### 3. Árbol de Merkle 🌳

Aprendemos cómo agrupar transacciones y verificar que una está incluida en un bloque sin descargar todo el bloque. Esta técnica es usada por los nodos livianos (SPV) en Bitcoin.

```python
import hashlib

def hash_sha256(data):
    return hashlib.sha256(data.encode()).hexdigest()

def hash_doble(a, b):
    return hashlib.sha256((a + b).encode()).hexdigest()

# Transacciones base
tx1 = hash_sha256("A")
tx2 = hash_sha256("B")
tx3 = hash_sha256("C")
tx4 = hash_sha256("D")

# Hash de pares
h12 = hash_doble(tx1, tx2)
h34 = hash_doble(tx3, tx4)

# Raíz de Merkle
raiz_merkle = hash_doble(h12, h34)
print("Raíz de Merkle:", raiz_merkle)
```

#### 🔍 Verificación de inclusión (Merkle Proof)

```python
def verificar_merkle(tx_hash, proof, raiz, posiciones):
    actual = tx_hash
    for i in range(len(proof)):
        if posiciones[i] == 'derecha':
            actual = hash_doble(actual, proof[i])
        else:
            actual = hash_doble(proof[i], actual)
    return actual == raiz

# Merkle proof para tx1
proof_tx1 = [tx2, h34]
posiciones_tx1 = ['derecha', 'derecha']

# Verificamos si tx1 está en la raíz
es_valido = verificar_merkle(tx1, proof_tx1, raiz_merkle, posiciones_tx1)
print("¿tx1 está en el árbol?:", es_valido)
```

---

## 🎓 Créditos

Este material forma parte de una serie educativa para entender cómo funciona la tecnología detrás de blockchain, sin necesidad de conocimientos previos en criptografía.


---

¡Abrí el notebook y jugá con los ejemplos! 🚀

---

🇦🇷🧉 BlockchainCriollo 