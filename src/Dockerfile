FROM mcr.microsoft.com/dotnet/aspnet:9.0.0-preview.1-alpine3.19 AS base
USER app
WORKDIR /app
ENV ASPNETCORE_URLS http://*:8080
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:9.0.0-preview.1-alpine3.19 AS build
WORKDIR /src
COPY ["rinha-dotnet-9.csproj", "."]
RUN dotnet restore "./rinha-dotnet-9.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "rinha-dotnet-9.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "rinha-dotnet-9.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "rinha-dotnet-9.dll"]


#docker build -t rinha-de-backend-2024-q01-dotnet9 .
#docker run -e "ConnectionStrings__rinha=Host=localhost;Database=rinha;Username=user;Password=password;Pooling=true;Minimum Pool Size=0;Maximum Pool Size=100;" -d -p 9999:9999 rinha-de-backend-2024-q01-dotnet 