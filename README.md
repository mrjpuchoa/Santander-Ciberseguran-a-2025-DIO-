# Santander Cibersegurança 2025 (DIO)
Este repositório tem o objetivo de apresentar os desafios propostos pelo bootcamp Santander Cibersegurança 2025 (DIO).

Para o desafio, escolhi realizar uma sala na plataforma TryHackMe. Sou usuário premium e estudo segurança ofensiva na plataforma há algum tempo. O desafio consiste em um Capture The Flag (CTF), que testa técnicas como enumeração e reconhecimento de portas com a ferramenta nmap, testes de invasão em um serviço FTP por força bruta (utilizei a ferramenta Hydra) e requisições HTTP via telnet. Observação: reiniciei a máquina do desafio algumas vezes devido a problemas técnicos, o que provocou alteração do IP do alvo em algumas ocasiões, mas isso não comprometeu a conclusão do desafio.

<img width="812" height="465" alt="01" src="https://github.com/user-attachments/assets/dfba3844-dff1-45a4-a84d-1a6337abfe44" />
<img width="1443" height="727" alt="03" src="https://github.com/user-attachments/assets/35f1e959-c463-4590-8ea2-56599044ef47" />

Os primeiros passos foram o reconhecimento das portas abertas no alvo.
Primeira pergunta: Qual a porta mais alta que está aberta antes da porta 10000?

<img width="1310" height="500" alt="04" src="https://github.com/user-attachments/assets/7d8d01f7-9557-4009-939a-bd90f075a96d" />

Realizei um scan das portas de 1 a 10000 conforme mostrado na imagem. A resposta é a porta 8080.

<img width="670" height="320" alt="image" src="https://github.com/user-attachments/assets/82c5613c-05da-4a66-9b4e-73c3e34df390" />

Para a segunda questão, realizei um scan das portas de 10000 a 11000 para localizar uma porta aberta que não estivesse entre as 1000 portas mais comuns e que ficasse acima da porta 10000. Encontrei: porta 10021.
Na terceira questão, a resposta foi: 6 portas abertas no IP alvo, no total.

<img width="667" height="216" alt="image" src="https://github.com/user-attachments/assets/ca032631-3a81-4daf-b78b-67b5309a615c" />

O próximo passo foi descobrir as flags escondidas. Uma delas estava no cabeçalho do servidor HTTP do alvo — para encontrá-la fiz uma requisição HTTP via telnet na porta 80 (porta padrão do HTTP). A outra flag estava no cabeçalho do servidor SSH — para identificá-la utilizei o argumento -sV do nmap para enumerar serviços e versões. Isso foi suficiente para encontrá-la.
Também foi identificado um serviço FTP rodando em porta não padrão. Por padrão, o FTP utiliza a porta 21, mas nos scans iniciais nenhum FTP foi detectado. Lembrei da porta 10021 (serviço desconhecido) e escaneei novamente para enumerar a versão do serviço — tratava-se do serviço FTP. Com isso, resolvi mais três questões do desafio: localização de duas flags e a versão do servidor FTP.

<img width="1273" height="318" alt="05" src="https://github.com/user-attachments/assets/7d46cd15-7439-49f8-97cc-9df4d545752b" />
<img width="678" height="497" alt="image" src="https://github.com/user-attachments/assets/66858df1-3d25-4ed9-bd02-192a70c083f1" />
<img width="1044" height="246" alt="image" src="https://github.com/user-attachments/assets/15f8ac8e-6a00-4286-8397-05433c000388" />
<img width="951" height="234" alt="11" src="https://github.com/user-attachments/assets/a1e015d7-5d60-4ec7-b47b-917f86989ccb" />

O objetivo seguinte foi fazer brute force no serviço FTP. Utilizei Hydra e a wordlist rockyou.txt (mais de 14 milhões de senhas). O desafio já pressupunha que eu havia obtido, por engenharia social, dois usernames: eddie e quinn — estes foram os alvos do ataque de força bruta. O comando utilizado foi:

hydra -l eddie -P /usr/share/wordlists/rockyou.txt -s 10021 ftp://10.201.126.254
hydra -l quinn -P /usr/share/wordlists/rockyou.txt -s 10021 ftp://10.201.126.254

Como o FTP não estava na porta padrão, foi necessário usar o argumento -s 10021 para especificar a porta descoberta.

<img width="1280" height="220" alt="06" src="https://github.com/user-attachments/assets/0298e8cc-42f6-44c7-8843-2f06af72555d" />
<img width="1040" height="232" alt="image" src="https://github.com/user-attachments/assets/6e7449a7-b6a1-4890-8d21-63c9acfa3ce1" />
<img width="1062" height="272" alt="13" src="https://github.com/user-attachments/assets/3f2e769a-cdcf-4c1d-be6a-c465b6589c1f" />

Com as senhas obtidas, autentiquei-me com ambos os usuários no serviço FTP e localizei a flag dentro do diretório do usuário quinn. Listei arquivos e pastas com ls, identifiquei o arquivo que continha a flag e usei get [arquivo] para baixar o arquivo para o meu computador. No próprio cliente FTP, utilizei !cat [arquivo] para exibir o conteúdo do arquivo no shell local — o prefixo ! permite executar comandos no shell sem encerrar a sessão FTP.

<img width="491" height="309" alt="14" src="https://github.com/user-attachments/assets/d16b610d-161d-471f-9242-51264a9cab14" />
<img width="696" height="314" alt="image" src="https://github.com/user-attachments/assets/46f8b272-829c-438c-8997-adf387939309" />
<img width="1039" height="479" alt="image" src="https://github.com/user-attachments/assets/d83c7ba6-dc45-4849-bfc6-921d667bd73a" />

O último desafio consistiu em fazer um scanner altamente furtivo que não fosse identificado pelo sistema IDS. Para isso, utilizei o nmap com o argumento -sN (Null scan), que não define flags no cabeçalho TCP. Esse tipo de scan pode gerar falta de resposta do servidor, o que pode indicar portas abertas ou interferência do IDS no bloqueio dos pacotes.

<img width="680" height="329" alt="17" src="https://github.com/user-attachments/assets/db3ce815-5388-4b4e-998e-849ad00c8ea4" />
<img width="571" height="388" alt="18" src="https://github.com/user-attachments/assets/656a7159-95ff-4b68-b4bc-8b432a915329" />

Com isso, garanti a última flag e concluí o desafio. Todas as respostas e evidências estão registradas; no final deixei o link para a sala concluída.

<img width="1272" height="851" alt="19" src="https://github.com/user-attachments/assets/9f149222-8b35-4dcf-ae02-dc0459109aad" />
<img width="988" height="619" alt="20" src="https://github.com/user-attachments/assets/01a38787-8e18-44b6-a37c-cf67ec597118" />

https://tryhackme.com/room/netsecchallenge?sharerId=6824bf6acd0a5c2008dfd0ca




