#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
#Prueba de CI/CD con jenkins

#Utilizar imagen de runtime compatible con netcore 3.1 en Debian 9 stretch para ejecutar el proyecto una vez que sea compilado.
FROM kdsoftio/dotnet-runtime AS base
WORKDIR /app
EXPOSE 80

#Utilizar imagen de SDK compatible con netcore 3.1 en Debian 9 stretch (Esta imagen contiene las herramientas y dependencias para para ejecutar y compilar la aplicacion)
FROM kdsoftio/dotnet-sdk AS build

#Workdir es el ecomando que utilizamos para ingresar a los directorios. En este caso ingresamos al dirctorio /src
WORKDIR /src

#En la siguente instruccion se copia el archivo del proyecto 
COPY ["Sigal-WebAPI3.1.csproj", ""]

#El sigueinte paso ejecuta el comando dotnet restore para restaurar paquetes nuget que utiliza la aplicacion.
RUN dotnet restore "./Sigal-WebAPI3.1.csproj"

#copiar todo el contenido de la aplicacion 
COPY . .

#Ingresar en el directorio /src
WORKDIR "/src/."

#Construir proyecto con sus dependencias en carpeta build
RUN dotnet build "Sigal-WebAPI3.1.csproj" -c Release -o /app/build

#Publicar la aplicacion y sus dependencia en la carpeta publish 
FROM build AS publish
RUN dotnet publish "Sigal-WebAPI3.1.csproj" -c Release -o /app/publish

#Las siguientes instruccions son para la ejecucion del proyecto basandose en la imagen que se definidia de runtiem al principio del dockerfile.
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Sigal-WebAPI3.1.dll"]


