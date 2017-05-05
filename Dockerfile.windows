FROM microsoft/dotnet-nightly:2.0-sdk-nanoserver

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ENV NUGET_XMLDOC_MODE skip
ENV DOTNET_SKIP_FIRST_TIME_EXPERIENCE 1

RUN New-Item -Path \MusicStore\samples\MusicStore -Type Directory
WORKDIR app

ADD samples/MusicStore/MusicStore.csproj samples/MusicStore/MusicStore.csproj
ADD build/dependencies.props build/dependencies.props
ADD NuGet.config .
RUN dotnet restore --runtime win10-x64 .\samples\MusicStore

ADD samples samples

RUN dotnet publish --output /out --configuration Release --framework netcoreapp2.0 --runtime win10-x64 .\samples\MusicStore

FROM microsoft/dotnet-nightly:2.0-runtime-nanoserver

WORKDIR /app
COPY --from=0 /out .

EXPOSE 5000
ENV ASPNETCORE_URLS http://0.0.0.0:5000

CMD dotnet musicstore.dll
