
<div> 
<p><a href="https://github.com/JosiTubaroski/Sircoi_Geral">Home</a></p>
</div> 

## 💡 DTEXEC

### 🟢 Perguntas:

 1. O que é o `dtexec` que geralmente fica no `C:\Program Files\Microsoft SQL Server\160\DTS\Binn`?

O `dtexec` é uma <b>ferramenta de linha de comando da Microsoft</b> usada para <b>executar pacotes do SQL Server Integration Services (SSIS)</b>.

👉 Ele geralmente fica nesse caminho que você mencionou:
    
<img width="431" height="84" alt="image" src="https://github.com/user-attachments/assets/92274893-6088-4f6e-b757-5f3ead6c2a79" />

(`160` corresponde à versão do SQL Server 2019/2022).

### Para que serve o dtexec?

- Executar pacotes SSIS (.dtsx) que foram desenvolvidos no <b>SQL Server Data Tools (SSDT)</b> ou no <b>Visual Studio</b>.
- Rodar pacotes armazenados no <b>MSDB</b> ou no <b>SSISDB (Catálogo do Integration Services)</b>.
- Executar pacotes salvos em arquivos (`.dtsx`) no sistema de arquivos.
- Permitir passar parâmetros, variáveis e opções de log pela linha de comando.

### Exemplos de uso

1. <b>Executar pacote salvo em arquivo</b>:

   <img width="287" height="78" alt="image" src="https://github.com/user-attachments/assets/90fea1df-ee95-4c93-a95a-ee08f118d0be" />



