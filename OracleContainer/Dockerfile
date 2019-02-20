FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app

LABEL io.k8s.display-name="Oracle Container" \
      io.k8s.description="Oracle Container" \
      io.openshift.expose-services="8080:http"; 

EXPOSE 8080
ENV ASPNETCORE_URLS=http://*:8080

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["OracleContainer/OracleContainer.csproj", "OracleContainer/"]
RUN dotnet restore "OracleContainer/OracleContainer.csproj"
COPY . .
WORKDIR "/src/OracleContainer"
RUN dotnet build "OracleContainer.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "OracleContainer.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "OracleContainer.dll"]