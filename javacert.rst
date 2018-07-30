===========================
Importar certificado na JVM
===========================

Quando um certificado é self-sign, ou seja, foi assinado por uma CA próprio, a jvm não irá 'confiar' nesse certificado.
Com isso ocorre o erro abaixo:

.. code-block:: console

    Caused by: feign.RetryableException: sun.security.validator.ValidatorException: PKIX path validation failed: java.security.cert.CertPathValidatorException: signature check failed executing GET https://<dominio>/

Para solucionar esse problema, uma das opções é importar o certificado na keystore da jvm.

Importando o certificado
========================

Via ansible
-----------

Se possuir o ansible instalado, com apenas a execução abaixo é possível importar o certificado:

.. code-block:: console

    $ ansible <host> -mjava_cert -a"executable=/usr/lib/jvm/jdk8/bin/keytool cert_url=<dominio> keystore_path=/usr/lib/jvm/jdk8/jre/lib/security/cacerts keystore_pass=changeit" --become

Manual
------

1. Extraia o certificado com o comando abaixo.

.. code-block:: console

    $ echo | openssl s_client -connect <dominio>:443

2. Copie e cole todo o conteúdo entre `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----` inclusive, em um arquivo.

3. Importe o certificado com o comando abaixo.

.. code-block:: console

    $ keytool -import -alias person.services.intranet -keystore /usr/java/jre6/lib/security/cacerts -file <arquivo> -storepass changeit


Testando a importação do certificado
====================================

A forma mais fácil de testar se um certificado foi importado com sucesso é atráves dessa aplicação java:

1. Copie e cole o código abaixo em um arquivo `SSLPoke.java`:

.. literalinclude:: SSLPoke.java
    :language: java

2. Compile o código com o comando:

.. code-block:: console

    $ javac SSLPoke.java

3. Execute o comando abaixo, se o resultado for `Successfully connected`, então o certificado foi importado com sucesso:

.. code-block:: console

    $ java SSLPoke <dominio> <port>
