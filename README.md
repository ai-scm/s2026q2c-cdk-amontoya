# AWS CDK con TypeScript

## Prerrequisitos

Antes de comenzar es necesario tener instaladas las siguientes herramientas:

### Node.js y npm

Verificar instalación:

```bash
node --version
npm --version
```

### AWS CDK

Instalar la CLI de AWS CDK:

```bash
npm install -g aws-cdk
```

Verificar instalación:

```bash
cdk --version
```

### AWS CLI

Verificar instalación:

```bash
aws --version
```

---

# 1. Creación del proyecto

Crear un nuevo proyecto CDK en TypeScript:

```bash
mkdir s2026q2c-cdk-amontoya
cd s2026q2c-cdk-amontoya

cdk init app --language typescript
```

Instalar dependencias:

```bash
npm install
```

La estructura generada es similar a:

```text
.
├── bin
├── lib
├── test
├── package.json
├── cdk.json
└── tsconfig.json
```

* `bin/`: punto de entrada de la aplicación.
* `lib/`: definición de la infraestructura.
* `test/`: pruebas.

---

# 2. Configuración de credenciales AWS

Es necesario tener AWS CLI instalado previamente.

Copiar las credenciales proporcionadas por AWS en:

```text
~/.aws/credentials
```

Ejemplo:

```ini
[xxxxxxxxx_PS-PowerUserAccess]
aws_access_key_id=XXXXXXXXXXXX
aws_secret_access_key=XXXXXXXXXXXX
aws_session_token=XXXXXXXXXXXX
```

Verificar que las credenciales funcionan:

```bash
aws sts get-caller-identity --profile xxxxxxxxx_PS-PowerUserAccess
```

---

# 3. Configuración del Bootstrap

CDK requiere una infraestructura mínima de soporte para poder desplegar recursos.

Ejecutar:

```bash
cdk bootstrap --profile xxxxxxxxx_PS-PowerUserAccess
```

Este comando crea el stack:

```text
CDKToolkit
```

que contiene los recursos necesarios para que CDK funcione correctamente.

---

# 4. Construimos nuestra aplicación con CDK

La aplicación se compone principalmente de:

* Un Stack.
* Una función Lambda.
* Un Bucket S3.

Los stacks disponibles pueden visualizarse mediante:

```bash
cdk list
```

o

```bash
cdk ls
```

---

# 5. Definimos una URL para la Lambda

Para exponer la función Lambda mediante una URL pública:

```typescript
const myFunctionUrl = myFunction.addFunctionUrl({
    authType: lambda.FunctionUrlAuthType.NONE,
});
```

También se puede mostrar la URL al finalizar el despliegue:

```typescript
new cdk.CfnOutput(this, "myFunctionUrlOutput", {
    value: myFunctionUrl.url,
});
```

---

# 6. Synthesize a CloudFormation Template

CDK transforma el código TypeScript en una plantilla CloudFormation.

Generar la plantilla:

```bash
cdk synth
```

Este comando permite visualizar la infraestructura que será desplegada sin crear recursos en AWS.

---

# 7. Deploy

Desplegar la infraestructura:

```bash
cdk deploy --profile 8xxxxxxxxx_PS-PowerUserAccess
```

Alternativamente, se puede exportar el perfil:

```bash
export AWS_PROFILE=xxxxxxxxx_PS-PowerUserAccess
```

y posteriormente ejecutar:

```bash
cdk deploy
```

Una vez finalizado el despliegue, los recursos estarán disponibles en AWS.

---

# 8. Modificar la aplicación

Posteriormente se modificó el Stack para incluir:

* Un bucket S3.
* Un archivo de texto llamado `hola.txt`.
* Despliegue automático del archivo mediante `BucketDeployment`.

Estructura:

```text
assets/
└── hola.txt
```

Contenido:

```text
hola mundo
```

Antes de desplegar cambios se pueden visualizar las diferencias:

```bash
cdk diff
```

Esto permite revisar qué recursos serán creados, modificados o eliminados.

Luego se aplican los cambios:

```bash
cdk deploy --profile xxxxxxxxxPS-PowerUserAccess
```

---

# 9. Destruir los recursos

Para eliminar toda la infraestructura creada:

```bash
cdk destroy --profile xxxxxxxxx_PS-PowerUserAccess
```

CDK solicitará confirmación antes de eliminar los recursos.

Una vez completado el proceso, el Stack y todos los recursos asociados serán eliminados de AWS.
