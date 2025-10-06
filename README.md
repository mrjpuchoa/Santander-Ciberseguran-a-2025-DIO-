# Santander-Ciberseguran-a-2025-DIO-
Este repositório tem o intuito de apresentar os desafios propostos pelo bootcamp

https://tryhackme.com/room/netsecchallenge?sharerId=6824bf6acd0a5c2008dfd0ca

Para o desafio eu escolhi realizar um challenge na plataforma TryHackMe.com. Este desafio consiste em um capture the flag (CTF), no qual testa técnicas como enumeração e reconhecimento de portas usando a ferramenta nmap, realizar testes de invasão em um serviço de protocolo FTP através de força bruta (utilizei a ferramenta Hydra para o brute force) e . Obs.: Resetei a máquina do desafio algumas vezes devido a problemas técnicos, o que provocou a mudança de ip do alvo algumas vezes, mas isso não atrapalha a conclusão do desafio.

<img width="812" height="465" alt="01" src="https://github.com/user-attachments/assets/dfba3844-dff1-45a4-a84d-1a6337abfe44" />
<img width="1443" height="727" alt="03" src="https://github.com/user-attachments/assets/35f1e959-c463-4590-8ea2-56599044ef47" />

Os primeiros passos são fazer o reconhecimento de portas que estão abertas no alvo.
A primeira pergunta: Qual a porta mais alta que está aberta antes da porta 10000?
<img width="1310" height="500" alt="04" src="https://github.com/user-attachments/assets/7d8d01f7-9557-4009-939a-bd90f075a96d" />
Fiz um scaner desde a porta 1 até a 10000 utilizando os argumentos como na imagem.
Resposta é porta 8080.
<img width="670" height="320" alt="image" src="https://github.com/user-attachments/assets/82c5613c-05da-4a66-9b4e-73c3e34df390" />
Para a segunda questão, realizei um escaner da porta 10000 até a 11000 para ver se seria o suficiente para encontrar a porta aberta que não está nas 1.000 portas comuns e que se encontrava acima da porta 10.000. Bingo! A resposta é a porta 10021
