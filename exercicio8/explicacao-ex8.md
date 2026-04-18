Para realizar essa verificação, foram necessários os seguintes passos técnicos:

Transferência de Identidade: Foi utilizado o comando scp na VM 1 para enviar não apenas o certificado (aluno.crt), mas também a chave privada (aluno.key) para o diretório /etc/ssl/certificates/ na VM 2 utilizando o comando scp aluno.key user@192.168.0.20:/etc/ssl/certificates/.

Implementação do Servidor HTTPS: Foi criado um script em Python (server.py) na VM 2 utilizando o módulo ssl. Esse script foi configurado para carregar o par de chaves e "envelopar" o socket do servidor, transformando uma conexão HTTP simples em uma conexão criptografada (HTTPS) na porta 8443. 
conteúdo do script server.py:
"
import http.server
import ssl

# Define o endereço e a porta
server_address = ('0.0.0.0', 8443)

# Cria o servidor HTTP básico
httpd = http.server.HTTPServer(server_address, http.server.SimpleHTTPRequestHandler)

# Cria o contexto SSL e carrega o certificado e a chave
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain(certfile='aluno.crt', keyfile='aluno.key')

# Envolve o socket do servidor com a camada de segurança SSL
httpd.socket = context.wrap_socket(httpd.socket, server_side=True)

print("Servidor HTTPS rodando em https://192.168.0.20:8443")
httpd.serve_forever()
"

Explicação:
A validação do certificado no navegador comprova a correta implementação da camada de segurança SSL/TLS entre as máquinas virtuais, demonstrando que o servidor na VM 2 está utilizando com sucesso o certificado digital gerado na VM 1. Embora o navegador exiba um alerta de "Potencial risco de segurança", isso ocorre exclusivamente porque o certificado é autoassinado (não possui a assinatura de uma Autoridade Certificadora externa confiável), mas o erro MOZILLA_PKIX_ERROR_SELF_SIGNED_CERT confirma que o aperto de mão (handshake) criptográfico aconteceu e que o navegador recebeu a identidade digital "aluno" configurada durante o exercício.