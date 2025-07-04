---

  

---

### 🔁 Fundamentos de la Programación Reactiva

La programación reactiva se basa en el **patrón observador** y el modelo **publicador-suscriptor**. La especificación Reactive Streams define reglas claras para esta interacción.

---
### 📌 Pasos Clave del Flujo Reactivo

#### **Paso 1: Suscripción**

- Hay dos entidades: **Publisher** (publicador) y **Subscriber** (suscriptor).
- El suscriptor se registra al publicador usando el método `subscribe()`.

```java
public interface Publisher<T> {
    void subscribe(Subscriber<? super T> subscriber);
}
```

```java
public interface Subscriber<T>
    void onSubscribe(Subscription subscription);
	void onNext(T item);
	void onError(Throwable throwable);
	void onComplete();
}
```
#### **Paso 2: Entrega del objeto Subscription**

- El publicador entrega un objeto `Subscription` al suscriptor.
- Este objeto permite al suscriptor **pedir datos** o **cancelar la suscripción**.


```java
public interface Subscription<T> {
    void request(long n);
	void cancel();
}
```

#### **Paso 3: Comunicación**

- El suscriptor usa el objeto `Subscription` para pedir una cantidad específica de ítems.
- El publicador responde usando el método `onNext()` para enviar los datos uno por uno.

```java
public interface Subscriber<T>
	void onNext(T item);
}
```

#### **Regla importante**:

- El publicador **nunca debe enviar más ítems de los solicitados**.
- Puede enviar menos, pero **jamás más**.

#### **Paso 4: Finalización**

- Si el publicador ya no tiene más datos, llama a `onComplete()`.
- Esto **termina la relación**: el objeto `Subscription` ya no es válido.

```java
public interface Subscriber<T>
	void onComplete();
}
```

#### **Paso 5: Manejo de errores**

- Si ocurre un error, el publicador llama a `onError()`.
- Esto también **termina la relación**.

```java
public interface Subscriber<T>
	void onError(Throwable throwable);
}
```

---

### 🔄 Resumen de métodos

- `onNext()`: puede llamarse 0 o más veces.
- `onComplete()` o `onError()`: se llama **una sola vez**, y **nunca ambas**.

---

### 🧠 Terminología Alternativa

|Rol|También llamado como...|
|---|---|
|Publisher|Source, Observable, Upstream, Producer|
|Subscriber|Sink, Observer, Downstream, Consumer|
|Processor|Operator (actúa como ambos: pub/sub)|

---

### 🧩 Modelado en Programación Reactiva

- **Frontend (React)** → suscriptor
- **Backend (REST API)** → publicador
- **Microservicios**: un servicio puede ser publicador, otro suscriptor
- **Métodos dentro de una clase**: un método puede actuar como publicador, otro como suscriptor

> Cualquier componente que **emite datos** es un **publicador**.  
> Cualquier componente que **consume datos** es un **suscriptor**.

---