# RemotePS3 0.1.1-alpha

Cliente Android para experimentar o Uso remoto do PlayStation 3 pela rede local. O APK usa o protocolo PREMO no modo PSP. O PKG foi preparado para o PS3 com HEN. Ele pode exportar um perfil PSP existente ou, quando não houver um, pedir autorização para criar um perfil para o Android usando o serviço de cadastro do firmware.

Os instaladores e os arquivos de verificação estão em [Releases](https://github.com/lucasmourabsb/RemotePS3/releases/tag/v0.1.1-alpha).

## O que passou nos testes

| Componente | Teste realizado |
| --- | --- |
| PKG | Instalou e abriu no RPCS3. O serviço de cadastro dos firmwares 4.92 e 4.93 aceitou perfis novos e preservou os perfis existentes. No PS3 Slim com HEN, a versão com saída pelo FTP gerou um perfil no armazenamento interno. |
| APK | No Android emulado, o APK assinado importou o arquivo produzido pelo PKG. O servidor de teste transmitiu vídeo H.264 e MPEG-4 com áudio ATRAC3 e AAC. O app completou a reconexão solicitada pelo servidor, enviou botões e analógicos e voltou ao neutro quando o controle de teste foi removido. |
| Tela cheia e qualidade | Na 0.1.1-alpha, o emulador exibiu 480 × 272 a 30 FPS em H.264 e 432 × 240 a 15 FPS em MPEG-4 com decodificação por software. O servidor de teste confirmou as opções de vídeo enviadas pelo APK. |
| PS3 Slim com HEN e AYN Thor | A 0.1.0-alpha exibiu o menu do PS3 na Thor e aceitou os controles físicos. Houve quadros verdes e uma queda de sessão. A estabilidade da 0.1.1-alpha no aparelho ainda está em aberto. |

## Compatibilidade com jogos

O console bloqueou **Sonic The Hedgehog** e **Sonic & All-Stars Racing Transformed**. O segundo continuou sem rodar após um teste temporário que marcou o jogo como compatível com Uso remoto. O resultado confirmado em hardware é a transmissão e o controle do menu do PS3.

O PKG cuida do pareamento. Para ampliar a compatibilidade, uma linha de pesquisa é investigar o serviço de Uso remoto durante a execução dos jogos. Uma alternativa seria capturar e codificar o vídeo no PS3 com um módulo próprio e decodificá-lo no Android. Essa alternativa ainda exige medir o custo de memória e processamento no console. O teste atual não determina se um CFW completo seria necessário. [Manual do PlayStation 3](https://manuals.playstation.net/document/pb/ps3/current/remoteplay/remoteplay.html), [código do PS3HEN](https://github.com/PS3Xploit/PS3HEN/blob/master/payload/modulespatch.c)

## Primeiro teste no PS3 Slim e na Thor

1. Coloque os dois aparelhos na mesma rede local. Para o primeiro teste pelo roteador, siga a configuração documentada pela Sony e ligue o PS3 ao roteador com um cabo Ethernet. A Thor pode ficar no Wi‑Fi. O cenário com o PS3 ligado ao roteador pelo Wi‑Fi de 2,4 GHz ainda precisa ser verificado. [Uso remoto pelo roteador no manual da Sony](https://manuals.playstation.net/document/pb/ps3/current/remoteplay/remotewired.html)
2. Ative o HEN e instale `RemotePS3-Profile-Setup.pkg` pelo Gerenciador de pacotes. Abra **RemotePS3 Profile Setup** no menu Jogo.
3. Confirme a exportação de um perfil PSP existente ou, quando o PKG oferecer, a criação de um perfil novo. O cadastro usa um slot livre e aparece como **RemotePS3** em **Configurações → Configurações de uso remoto → Lista de dispositivos registrados**. O instalador salva `remoteps3-01.rps3` para o primeiro slot. Com um pendrive FAT32 conectado, o arquivo vai para o pendrive. Sem pendrive, copie o perfil por FTP da pasta `/dev_hdd0/game/RPS300001/USRDIR/`. Se já tiver um `export.orp` do Open Remote Play para esse console, você também pode usá-lo. [Registro de dispositivos no manual da Sony](https://manuals.playstation.net/document/pb/ps3/current/settings/registerdevice.html)
4. Copie o perfil para a Thor, instale o APK e toque em **Importar perfil**. O arquivo contém a chave do dispositivo registrado. Guarde-o com você e compartilhe apenas a mensagem de erro ou o resultado do teste.
5. No PS3, abra **Rede → Uso remoto**. Na Thor, toque em **Buscar PS3** ou digite o IPv4 que aparece nas configurações de rede do console. Toque em **Conectar**. [Uso remoto pela rede local no manual da Sony](https://manuals.playstation.net/document/pb/ps3/current/remoteplay/remotewired.html)
6. Se o menu aparecer na Thor, teste os direcionais, os botões, o áudio e depois um jogo compatível. Durante a transmissão, toque em **Menu → Desconectar** para fechar a sessão.

Se a busca automática não encontrar o PS3, digite o IP mostrado em **Configurações → Configurações de rede → Lista de configurações e status de conexão**. Se houver vídeo sem som, confira **Configurações → Configurações de uso remoto → Dispositivo de saída de áudio** e selecione a saída pelo dispositivo de Uso remoto. [Configurações de rede](https://manuals.playstation.net/document/pb/ps3/current/settings/statuslist.html), [saída de áudio](https://manuals.playstation.net/document/pb/ps3/current/settings/outputdevice.html)

O APK mantém o perfil durante o uso. Ao abrir o app novamente depois que o Android encerrá-lo, importe o mesmo arquivo. Para revogar o acesso, apague somente o perfil **RemotePS3** criado pelo instalador na lista de dispositivos registrados. [Lista de dispositivos registrados](https://manuals.playstation.net/document/pb/ps3/current/settings/devicelist.html)

## Qualidade do vídeo

Antes de conectar, toque na linha de qualidade para escolher a resolução, a taxa de quadros e a taxa de vídeo pedidas ao PS3. O modo PSP oferece no app 480 × 272, 432 × 240 e 320 × 240, com opção de 30 ou 15 FPS. O padrão é 480 × 272 a 30 FPS e 768 kb/s.

O vídeo ocupa a tela cheia e mantém a proporção da imagem. As estatísticas no canto mostram a resolução decodificada, os quadros entregues ao visor por segundo, a taxa de vídeo recebida e o codec. O FPS mede a transmissão na Thor. Toque em **Menu → Ocultar estatísticas** para esconder o painel.

Se a imagem ficar verde, desconecte e compare **Decodificador → Software** com **Automático**. A opção depende dos decodificadores disponíveis no Android. Se a sessão cair, anote a mensagem exibida ao voltar à tela de conexão.

## Compilar

### Android

Use JDK 17, Gradle 8.9, SDK Android 35 e NDK 27.3.13750724. O APK suporta `arm64-v8a` e `x86_64` a partir do Android 8.0. O código JNI usa uma compilação estática do decodificador ATRAC3 do FFmpeg.

```sh
tar -xf third_party/ffmpeg-7.1.2.tar.xz
export ANDROID_NDK_HOME="/caminho/para/ndk/27.3.13750724"
export FFMPEG_SOURCE="$PWD/ffmpeg-7.1.2"
android/native/build-atrac3.sh
gradle -p android :app:assembleDebug :app:lintDebug
```

O APK de desenvolvimento sai em `android/app/build/outputs/apk/debug/app-debug.apk`. Para distribuir uma compilação, gere `:app:assembleRelease` e assine o APK com a sua chave Android.

### PS3

Use o toolchain ps3dev, o runtime PSL1GHT do commit `6e565a70e927f55813babe86dc6f64149535f908` e o `prx-gen` do PS3DK v0.27.0. O build usa a nightly ps3dev de 26/07/2026. Para compilar o `prx-gen` a partir do repositório do PS3DK, rode `cargo build --locked -p prx-gen --manifest-path tools/Cargo.toml` no checkout dessa versão. [Toolchain ps3dev](https://github.com/ps3dev/ps3dev/releases/tag/nightly-2026-07-26), [runtime PSL1GHT](https://github.com/ps3dev/PSL1GHT/tree/6e565a70e927f55813babe86dc6f64149535f908), [PS3DK v0.27.0](https://github.com/FirebirdTA01/PS3DK/tree/v0.27.0)

```sh
PS3DEV="/caminho/para/ps3dev" PRX_GEN="/caminho/para/prx-gen" ps3/build.sh
```

O pacote sai em `ps3/build/RemotePS3-Profile-Setup.pkg`.

### Testes locais

```sh
mkdir -p build/tests
cc -Wall -Wextra -Werror tests/registry_test.c ps3/source/registry.c -o build/tests/registry
build/tests/registry
javac -d build/tests android/app/src/main/java/br/com/remoteps3/{Profile,PremoWire}.java tests/WireTest.java
java -ea -cp build/tests br.com.remoteps3.WireTest
```

Para o teste de vídeo, áudio e controle, instale o FFmpeg de linha de comando com libx264 e o pacote Python `pycryptodome`. Rode `python3 tests/mock_premo.py --profile build/tests/perfil.rps3`. Importe o perfil gerado e conecte ao servidor de teste. No emulador Android padrão, o endereço `10.0.2.2` aponta para a máquina que roda o emulador. Em aparelhos de teste na rede local, use `--bind` com o IPv4 da máquina de teste. `--restore-once` solicita uma segunda sessão.

Para validar a negociação de outro modo, use `--resolution 432x240 --fps 15` e selecione a mesma combinação no app. Confira a resolução e a taxa medidas no canto da tela.

Com a sessão aberta em um emulador Android que ofereça `uinput`, rode `adb shell uinput - < tests/controller_detach.json`. O servidor deve registrar movimento seguido de um comando neutro com os quatro eixos em `128`, quando o controle virtual for removido.

## Créditos e licenças

O formato do perfil PSP, o protocolo PREMO e as referências de sessão, mídia e controle vieram do Open Remote Play, de Darryl Sokoloski, no commit `5bf4590fc11d41f190bc7a8f6222f37038fe20ac`. A investigação sobre pareamento consultou o PsOldRemotePlay no commit `6b34090f53eece4ba88d90006430f21f86977c35`. [Open Remote Play](https://github.com/gbraad/open-rp), [PsOldRemotePlay](https://github.com/dsdmsa/PsOldRemotePlay)

O código do RemotePS3 usa GPL-2.0-or-later. Veja `COPYING`. O APK inclui partes do FFmpeg 7.1.2 sob a GPL por causa das opções de compilação. O código correspondente está em `third_party/ffmpeg-7.1.2.tar.xz`. O PKG usa o runtime PSL1GHT e arquivos de inicialização de módulo adaptados do PS3DK, sob licença MIT. As licenças estão em `third_party`. [FFmpeg 7.1.2](https://ffmpeg.org/releases/ffmpeg-7.1.2.tar.xz), [PSL1GHT](https://github.com/ps3dev/PSL1GHT), [PS3DK](https://github.com/FirebirdTA01/PS3DK)
