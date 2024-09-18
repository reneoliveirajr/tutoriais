## Tutorial de Manipulação de Chaves SSH

### 1. **Criação de uma Chave SSH**
O comando para criar uma chave SSH é o ssh-keygen. Ele gera um par de chaves: uma pública e outra privada.

#### Comando:
bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/my-key


- **-t rsa**: Tipo de algoritmo, neste caso RSA.
- **-b 4096**: Tamanho da chave em bits (quanto maior, mais seguro).
- **-f ~/.ssh/my-key**: Especifica o caminho e nome do arquivo da chave. Você pode mudar para outro local ou nome, como necessário.
  
#### Passos:
1. Será solicitado para definir uma senha para a chave privada (opcional, mas recomendado).
2. O comando gera dois arquivos:
   - **my-key** (chave privada): guarde com cuidado.
   - **my-key.pub** (chave pública): compartilhe essa com quem precisa se conectar.

### 2. **Proteção da Chave Privada**
No Linux, é importante limitar as permissões do arquivo da chave privada para garantir sua segurança.

#### Comando:
bash
chmod 400 ~/.ssh/my-key


- **400**: Somente leitura pelo dono do arquivo. Isso evita que outros usuários no sistema tenham acesso à chave.

### 3. **Conversão de Chaves SSH**
Ferramentas como **PuTTY** utilizam o formato PPK para as chaves. Portanto, se você criou uma chave no formato OpenSSH, é necessário convertê-la.

#### 3.1 Converter OpenSSH para PPK (com PuTTYgen)
1. **Abra o PuTTYgen**.
2. Clique em **"Load"** e selecione sua chave privada (formato .pem ou OpenSSH).
3. Após carregar a chave, clique em **"Save private key"** para salvar no formato **PPK**.

#### 3.2 Converter PEM para OpenSSH (via terminal)
Às vezes, você pode precisar converter uma chave PEM para o formato OpenSSH ou vice-versa.

##### Para converter PEM para OpenSSH:
bash
ssh-keygen -p -m PEM -f ~/.ssh/my-key


- **-p**: Modifica a senha da chave (pode deixá-la vazia).
- **-m PEM**: Especifica o formato de saída como PEM (padrão OpenSSH).
- **-f ~/.ssh/my-key**: O caminho do arquivo da chave.

### 4. **Copiando a Chave Pública para o Servidor**
Para facilitar a autenticação por chave, você pode copiar a chave pública para o servidor remoto.

#### Comando:
bash
ssh-copy-id -i ~/.ssh/my-key.pub user@remote-server


- **-i**: Especifica o caminho do arquivo de chave pública.
- **user@remote-server**: Usuário e servidor remoto onde a chave será adicionada.

### 5. **Ajustando Permissões no Windows com icacls**
Se você estiver no Windows e precisar ajustar as permissões de arquivo da chave privada, o comando é o icacls (equivalente ao chmod no Linux).

#### Comando para remover permissões herdadas e dar permissão de leitura apenas:
powershell
icacls .\my-key /inheritance:r
icacls .\my-key /grant:r "NOME_DO_SEU_USUARIO:(R)"


- **/inheritance:r**: Remove a herança de permissões.
- **/grant:r**: Concede permissão de leitura (R) apenas para o usuário atual.

### 6. **Laboratório Prático**
Aqui está um laboratório para você testar todos os comandos e fixar o aprendizado:

#### Passo 1: Criar uma chave SSH
- **Objetivo**: Gerar um par de chaves pública e privada.
bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/lab-key


#### Passo 2: Proteger a chave privada
- **Objetivo**: Aplicar as permissões corretas para a chave privada no Linux.
bash
chmod 400 ~/.ssh/lab-key


#### Passo 3: Converter uma chave para OpenSSH (opcional)
- **Objetivo**: Caso tenha uma chave em outro formato, como PEM, você pode convertê-la para o formato OpenSSH.
bash
ssh-keygen -p -m PEM -f ~/.ssh/lab-key


#### Passo 4: Testar a cópia da chave pública para um servidor remoto
- **Objetivo**: Usar ssh-copy-id para adicionar a chave pública no servidor remoto.
bash
ssh-copy-id -i ~/.ssh/lab-key.pub user@remote-server


#### Passo 5: Usar a chave para acessar o servidor
- **Objetivo**: Autenticar em um servidor remoto usando a chave privada.
bash
ssh -i ~/.ssh/lab-key user@remote-server


#### Passo 6: Ajustar permissões da chave no Windows
- **Objetivo**: Ajustar as permissões da chave privada no Windows usando icacls.
powershell
icacls .\lab-key /inheritance:r
icacls .\lab-key /grant:r "power\reneo:(R)"


#### Passo 7: Converter uma chave PEM para PPK (se aplicável)
- **Objetivo**: Usar o PuTTYgen para converter uma chave PEM para PPK e conectar com PuTTY.
