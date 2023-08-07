#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Arquivo dockerfile utilizado para construir a imagem do container da aplicação

# Instalando uma imagem para execução do .NET 7 (base)
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
# Define uma pasta de trabalho dentro do container
WORKDIR /app
# Porta utilizada pelo container
EXPOSE 80

# Compilando a imagem do .NET 7 (build)
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
# Define uma pasta de trabalho dentro do container
WORKDIR /src
# Copiando os arquivos necessários para execução do projeto
COPY ["UsersAPI.Services/UsersAPI.Services.csproj", "UsersAPI.Services/"]
COPY ["UsersAPI.Infra.IoC/UsersAPI.Infra.IoC.csproj", "UsersAPI.Infra.IoC/"]
COPY ["UsersAPI.Application/UsersAPI.Application.csproj", "UsersAPI.Application/"]
COPY ["UsersAPI.Domain/UsersAPI.Domain.csproj", "UsersAPI.Domain/"]
COPY ["UsersAPI.Infra.Data/UsersAPI.Infra.Data.csproj", "UsersAPI.Infra.Data/"]
COPY ["UsersAPI.Infra.Messages/UsersAPI.Infra.Messages.csproj", "UsersAPI.Infra.Messages/"]
COPY ["UsersAPI.Infra.Security/UsersAPI.Infra.Security.csproj", "UsersAPI.Infra.Security/"]
# Executa o comando restore do NuGet para instalar todas as bibliotecas mapeadas
# pelos arquivos .csproj copiados anteriormente
RUN dotnet restore "UsersAPI.Services/UsersAPI.Services.csproj"
# Copiando todos os arquivos gerados para dentro do container
COPY . .
# Define o projeto de inialização / execução
WORKDIR "/src/UsersAPI.Services"
# Compila (build) o projeto
RUN dotnet build "UsersAPI.Services.csproj" -c Release -o /app/build

# Executa (publish) o projeto
FROM build AS publish
RUN dotnet publish "UsersAPI.Services.csproj" -c Release -o /app/publish /p:UseAppHost=false

#finalizando e publicando o container
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "UsersAPI.Services.dll"]