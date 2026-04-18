# Projeto de Segurança de Redes - Implementação SSL/TLS

**Nome:** Giovanni Macedo Stenico RA: 202310105.
**Ambiente:** Duas VMs Debian no VirtualBox.

## Configuração de Rede
* **VM 1 (Cliente):** 192.168.0.10
* **VM 2 (Servidor):** 192.168.0.20

## Dificuldades
A parte mais complicada foi lidar com as permissões da VM 2. O comando SCP travava ao tentava enviar o certificado para uma pasta do sistema (`/etc/ssl/certificates/`) que o o usuário "user" não tinha direito de escrever. 

## Conclusão
O projeto serviu para o conhecimento do funcionamento da segurança entre duas máquinas na prática. Foi possivel o envio de arquivos na VM 1, passar para a VM 2 e fazer o navegador reconhecer o certificado, mesmo dando aquele aviso de segurança por ser autoassinado. No fim, foi observado como o SCP e o SSL trabalham juntos para proteger a comunicação.