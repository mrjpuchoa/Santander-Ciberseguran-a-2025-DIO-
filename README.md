# Santander Cibersegurança 2025 (DIO)
Este repositório tem o intuito de apresentar os desafios propostos pelo bootcamp

Para o desafio eu escolhi realizar um challenge na plataforma TryHackMe.com. Já sou usuário premium e estudo segurança ofensiva há algum tempo por meio da plataforma. Este desafio consiste em um capture the flag (CTF), no qual testa técnicas como enumeração e reconhecimento de portas usando a ferramenta nmap, realizar testes de invasão em um serviço de protocolo FTP através de força bruta (utilizei a ferramenta Hydra para o brute force) e utilização do telnet para requisições HTTP. Obs.: Resetei a máquina do desafio algumas vezes devido a problemas técnicos, o que provocou a mudança de ip do alvo algumas vezes, mas isso não atrapalha a conclusão do desafio.

<img width="812" height="465" alt="01" src="https://github.com/user-attachments/assets/dfba3844-dff1-45a4-a84d-1a6337abfe44" />
<img width="1443" height="727" alt="03" src="https://github.com/user-attachments/assets/35f1e959-c463-4590-8ea2-56599044ef47" />

Os primeiros passos são fazer o reconhecimento de portas que estão abertas no alvo.
A primeira pergunta: Qual a porta mais alta que está aberta antes da porta 10000?

<img width="1310" height="500" alt="04" src="https://github.com/user-attachments/assets/7d8d01f7-9557-4009-939a-bd90f075a96d" />

Fiz um scaner desde a porta 1 até a 10000 utilizando como mostra na imagem.
Resposta é porta 8080.

<img width="670" height="320" alt="image" src="https://github.com/user-attachments/assets/82c5613c-05da-4a66-9b4e-73c3e34df390" />

Para a segunda questão, realizei um escaner da porta 10000 até a 11000 para ver se seria o suficiente para encontrar a porta aberta que não está nas 1.000 portas comuns e que se encontrava acima da porta 10.000. Bingo! A resposta é a porta 10021. E na terceira questão a resposta é 6 portas estão abertas no ip alvo no total.

<img width="667" height="216" alt="image" src="https://github.com/user-attachments/assets/ca032631-3a81-4daf-b78b-67b5309a615c" />

Próximo passo é descobrir as flags escondidas, umas delas está no cabeçário do servidor HTTP do alvo, para encontrá-la eu fiz uma requisição HTTP através do telnet na porta 80 que é a porta padrão do protocolo HTTP, a outra flag está no cabeçário do servidor SSH, para encontrá-la eu usei o argumento -sV no nmap para enumerar os serviços e suas versões, isso foi o suficiente. Também foi citado que há um serviço FTP escutando em uma porta não padrão, por padrão a porta do protocolo FTP é a porta 21, e nos meus scans não foi detectado nenhum serviço FTP, mas lembra daquela porta 10021 com um serviço desconhecido, resolvi scaneá-la novamente para enumerar a versão do serviço e descobrir um pouco mais sobre o que está nesta porta, e acontece que é exatamente o serviço FTP. Assim pude resolver mais 3 questões do desafio que consiste em capturar duas flags e a versão do servidor FTP.

<img width="1273" height="318" alt="05" src="https://github.com/user-attachments/assets/7d46cd15-7439-49f8-97cc-9df4d545752b" />
<img width="678" height="497" alt="image" src="https://github.com/user-attachments/assets/66858df1-3d25-4ed9-bd02-192a70c083f1" />
<img width="1044" height="246" alt="image" src="https://github.com/user-attachments/assets/15f8ac8e-6a00-4286-8397-05433c000388" />
<img width="951" height="234" alt="11" src="https://github.com/user-attachments/assets/a1e015d7-5d60-4ec7-b47b-917f86989ccb" />

O objetivo agora é fazer o brute force no serviço FTP, para isso usei a ferramenta Hydra, utilizei a tão famosa wordlist 'rockyou.txt' que possui mais de 14 milhôes de senhas. o Desafio já suponha hipoteticamente que conseguiu dois usernames 'eddie' e 'quinn' por meio de engenharia social, os quais serão os alvos, para fazer o brute force utilizei o seguinte comando: hydra -l eddie -P /usr/share/wordlists/rockyou. txt -s 10021 ftp://10. 201. 126. 254
Como o FTP não está rodando na porta padrão, é necessário usar o argumento -s [porta 10021] que é a porta não padrão que foi descoberta rodando o serviço FTP.

<img width="1280" height="220" alt="06" src="https://github.com/user-attachments/assets/0298e8cc-42f6-44c7-8843-2f06af72555d" />
<img width="1040" height="232" alt="image" src="https://github.com/user-attachments/assets/6e7449a7-b6a1-4890-8d21-63c9acfa3ce1" />
<img width="1062" height="272" alt="13" src="https://github.com/user-attachments/assets/3f2e769a-cdcf-4c1d-be6a-c465b6589c1f" />

Com as senhas, apenas precisei entrar com ambos usuários e senhas no serviço FTP e encontrar a flag para concluir mais uma questão. A  flag estava no usuário 'quinn', listei os aqruivos e pasta com o comando 'ls' e lá estava o arquivo com a flag, usei o comando get [arquivo] para baixar o arquivo no meu desktop, então dentro do próprio servidor FTP eu posso usar o comando !cat [arquivo que foi baixado] para conseguir exibir a flag, vale ressaltar que o símbolo '!' auxilia para que eu possa injetar comandos no meu shell local, dessa forma estarei interagindo no meu shell local sem precisar deslogar da minha sessão no serviço FTP.

<img width="491" height="309" alt="14" src="https://github.com/user-attachments/assets/d16b610d-161d-471f-9242-51264a9cab14" />
<img width="696" height="314" alt="image" src="https://github.com/user-attachments/assets/46f8b272-829c-438c-8997-adf387939309" />
<img width="1039" height="479" alt="image" src="https://github.com/user-attachments/assets/d83c7ba6-dc45-4849-bfc6-921d667bd73a" />

O último desafio foi fazer um scanner altamente furtivo que não pudesse ser identificado pelo sistema IDS. Precisei apenas usar o nmap com o seguinte comando 'nmap sN [ip do alvo].
Este argumento -sN é um Null, consiste em não usar nenhuma flag no scanner. Contudo a falta de resposta do servidor pode indicar que as devidas portas estão abertas ou que o IDS ésta bloqueando os pacotes.

<img width="680" height="329" alt="17" src="https://github.com/user-attachments/assets/db3ce815-5388-4b4e-998e-849ad00c8ea4" />
<img width="571" height="388" alt="18" src="https://github.com/user-attachments/assets/656a7159-95ff-4b68-b4bc-8b432a915329" />

Então consegui garantir a última flag para concluir o desafio. Todas as questões realizadas, deixarei no final o link do meu desafio concluído.

<img width="1272" height="851" alt="19" src="https://github.com/user-attachments/assets/9f149222-8b35-4dcf-ae02-dc0459109aad" />
<img width="988" height="619" alt="20" src="https://github.com/user-attachments/assets/01a38787-8e18-44b6-a37c-cf67ec597118" />

https://tryhackme.com/room/netsecchallenge?sharerId=6824bf6acd0a5c2008dfd0ca




