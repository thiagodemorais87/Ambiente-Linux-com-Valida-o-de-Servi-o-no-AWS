## Projeto: Ambiente Linux com Validação de Serviço no AWS

Este documento descreve os passos necessários para configurar um ambiente Linux no AWS, instalar o servidor Nginx, criar e automatizar um script de validação de serviço, e versionar o código utilizando Git.

---

#### **1.. Atualizar o Sistema**
```bash
sudo apt update && sudo apt upgrade -y
```

---

### **2. Instalação do Servidor Nginx**

1. **Instalar o Nginx:**
   ```bash
   sudo apt install nginx -y
   ```
2. **Iniciar e habilitar o serviço Nginx:**
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```
3. **Verificar o funcionamento:**
   - No navegador, acesse o IP público da instância e confira se a página padrão do Nginx é exibida.

---

### **3. Criar o Script de Validação de Serviço**

1. **Criar o script:**
   - Salve o seguinte conteúdo em `/usr/local/bin/validar_servico.sh`:
     ```bash
     #!/bin/bash
     SERVICE="nginx"
     STATUS=$(systemctl is-active $SERVICE)
     DATE=$(date "+%Y-%m-%d %H:%M:%S")
     OUTPUT_DIR="/var/log/servico_status"
     ONLINE_FILE="$OUTPUT_DIR/online.log"
     OFFLINE_FILE="$OUTPUT_DIR/offline.log"

     mkdir -p $OUTPUT_DIR

     if [ "$STATUS" == "active" ]; then
         echo "$DATE - $SERVICE - ONLINE - Serviço em funcionamento." >> $ONLINE_FILE
     else
         echo "$DATE - $SERVICE - OFFLINE - Serviço não está em funcionamento." >> $OFFLINE_FILE
     fi
     ```

2. **Tornar o script executável:**
   ```bash
   sudo chmod +x /usr/local/bin/validar_servico.sh
   ```

---

### **4. Automatização do Script com Cron**

1. **Editar o Cron:**
   ```bash
   crontab -e
   ```
2. **Adicionar a tarefa automatizada:**
   ```bash
   */5 * * * * /usr/local/bin/validar_servico.sh
   ```

---

### **5. Configuração do Git e Versionamento**

1. **Instalar o Git:**
   ```bash
   sudo apt install git -y
   ```
2. **Configurar o Git:**
   ```bash
   git config --global user.name "Seu Nome"
   git config --global user.email "seuemail@example.com"
   ```
3. **Inicializar o Repositório:**
   ```bash
   git init
   ```
4. **Adicionar os Arquivos:**
   ```bash
   git add validar_servico.sh
   ```
5. **Fazer o Commit:**
   ```bash
   git commit -m "Adiciona script de validação do Nginx"
   ```
6. **Subir para o Repositório Remoto:**
   - Crie um repositório no GitHub.
   - Adicione o repositório remoto:
     ```bash
     git remote add origin <URL-do-repositório>
     ```
   - Envie os commits:
     ```bash
     git push -u origin main
     ```

---

### **6. Documentação Adicional**

- Certifique-se de manter as configurações de segurança da instância EC2 atualizadas.
- Para verificar os logs gerados pelo script, acesse o diretório `/var/log/servico_status/`.
- Monitorar o desempenho da instância EC2 pelo console AWS para ajustes, se necessário.

---

**Contato:**
- Caso precise de suporte, entre em contato pelo e-mail: thiagomgoncalves87@gmail.com

