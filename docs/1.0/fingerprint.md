---
layout: page-classic-sidebar-left
title: Configuração do Fingerprint
featimg: MapAntifraud.png
previous: /docs/1.0/postnotification
next: /docs/1.0/autenticacao
---

Serviço que coleta impressão digital de dispositivos e geolocalização real do IP do comprador.  

-----------------------------------

## * ReDShield

## Integração com sua página de checkout(site)

## Como funciona?

![Fluxo]({{ site.url }}/img/fingerprint.png){: .centerimg }{:title="Fluxo da coleta do fingerprint "}

1 - A página de checkout da loja envia os atributos do dispositivo do comprador para a Iovation, criando assim a *caixa preta*  
2 - O lojista recebe a sequência de caracteres criptografados da Iovation e escreve o mesmo na página de checkout em um campo do tipo *hidden*  
3 - O lojista envia para a Braspag, junto com os demais dados da transação a ser analisada, a *caixa preta*  
4 - A Braspag recebe todos os dados, valida e envia para a ReD Shield  
5 - A ReD Shield recebe todos os dados, envia a *caixa preta* para a Iovation descriptografar  
6 - A Red Shield recebe da Iovation os atributos do dispositivo do comprador 

## Como configurar?

1 - Inclua o javascript da Iovation em sua página de checkout  
2 - Adicione parâmetros de configuração no javascript  
3 - Crie um campo do tipo *hidden* em sua página para escrever a *caixa preta* nele e enviá-lo junto com os dados da transação a ser analisada  

**Obs.:** Não realize cache do script, pois pode ocorrer de vários dispositovos sejam identificados como sendo o mesmo.

* Incluindo o javascript da Iovation  

    Para incluir o javascript, adicione o seguinte elemento `<script>` na sua página de checkout. Esta é a URL da versão do snare.js do ambiente de produção da Iovation.  
    **Exemplo:** `<script type="text/javascript" src="https://mpsnare.iesnare.com/snare.js"></script>`  

* Parâmetros de configuração

    **io_install_flash**{:.custom-attrib} `optional`{:.custom-tag} `bool`{:.custom-tag} `DefaultValue=false`{:.custom-tag}  
    Determina se será solicitado ao usuário a instalação do Flash ou atualização da versão.  

    **io_flash_needs_handler**{:.custom-attrib} `optional`{:.custom-tag}  
    Este parâmetro só terá validade se o parâmetro *io_install_flash* estiver configurado como TRUE, caso contrário não será executado.  
    É possível aqui customizar sua própria mensagem caso o Flash não esteja instalado.  
    Ex.: var io_flash_needs_handler = "Alert('Instalar Flash');"

    **io_install_stm**{:.custom-attrib} `optional`{:.custom-tag} `bool`{:.custom-tag} `DefaultValue=false`{:.custom-tag}  
    Determina se será solicitado ao usuário a instalação do Active X, que ajuda a coletar informações do hardware.  
    Este controle está disponível somente para o Internet Explorer, e caso o Active X já se encontre instalado, esta configuração não terá efeito.

    **io_exclude_stm**{:.custom-attrib} `optional`{:.custom-tag} `bitmask`{:.custom-tag}  `DefaultValue=15`{:.custom-tag}   
    Determina se o Active X deverá ser executado quando instalado. É possível optar por desativar o controle para plataformas específicas.  
    Possíveis valores:  
        0 - executa em todas as plataformas  
        1 - não executa no Windows 9.x (incluindo as versões 3.1, 95, 98 e ME)  
        2 - não executa no Windows CE  
        4 - não executa no Windows XP (incluindo as versões NT, 2000, 2003 e 8)  
        8 - não executa no Windows Vista  
        
    Obs.: Os valores são combinação de somas dos valores acima, por exemplo: 12 - não executa no Windows XP (4) ou no Windows Vista (8).

    **io_bbout_element_id**{:.custom-attrib} `optional`{:.custom-tag}  
    Id do elemento HTML para preencher com a *caixa preta*. Se o parâmetro *io_bb_callback* for definido, este não terá efeito.  

    **io_enable_rip**{:.custom-attrib} `optional`{:.custom-tag} `bool`{:.custom-tag} `DefaultValue=true`{:.custom-tag}  
    Determina se tentará coletar informações para obter o IP real do usuário.  

    **io_bb_callback**{:.custom-attrib} `optional`{:.custom-tag}  
    Parâmetro para customizar a checagem da coleta da *caixa preta* foi concluída.  
    Ao utilizar, escrever a função conforme com a seguinte sintaxe:  
    *io_callback(bb, complete)*, onde:  
        bb - valor da caixa preta  
        complete - valor booleano que indica que a coleta foi concluída  

    **IMPORTANTE!**  
    Os parâmetros de configuração devem ser colocados antes da chamada da tag acima. Eles determinam como javascript do iovation funcionará, e podem ocorrer erros
    caso os mesmos sejam colocados antes da chamada do javascript.

**Exemplo**  

```html
<html>
<head>
<script>
    var io_install_flash = false;
    var io_install_stm = false;
    var io_bbout_element_id = 'gatewayFingerprint';
</script>
</head>
<script type="text/javascript" src="https://mpsnare.iesnare.com/snare.js"></script>

<body>
    <form>
        <input type="hidden" name="gatewayFingerprint" id="gatewayFingerprint">
    </form>
</body>
</html>
```

## Integração em aplicativos mobile

**Visão Geral**  
Este tópico explica como integrar o mobile SDK da Iovation em seus aplicativos para iOS e Android.  

**Baixando o SDK**  
Se você ainda não baixou o SDK do iOS ou do Android, deve fazê-lo antes de continuar através do centro de ajuda da Iovation.  
[Download Mobile SDK](http://help.iovation.com/Downloads)

**Sobre a integração**  
Adicione o Iovation Mobile SDK aos seus aplicativos para coletar informações sobre os dispositivos dos usuários finais. Será gerada uma *caixa preta* que contém todas as informações do dispositivo disponíveis.

![Fluxo]({{ site.url }}/img/fingerprintmobile.png){: .centerimg }{:title="Fluxo da coleta do fingerprint mobile"}  

* Integrando com aplicativos iOS  

Arquivos e requisitos de integração do iOS  
![Detalhes]({{ site.url }}/img/fingerprintios1.png){: .left }{:title="Detalhes integração iOS"}  

Esta versão suporta iOS 5.1.1 ou superior nos seguintes dispositivos:  
        iPhone 3GS e posterior  
        iPod Touch 3ª geração ou posterior  
        Todos os iPads  

* Instalando o SDK no iOS  

    1 - Baixe e descompacte o SDK  
    2 - No Xcode, arraste *iovation.framework* na área de navegação do seu projeto  
![Detalhes]({{ site.url }}/img/fingerprintios2.png){: .left }{:title="Detalhes instalação SDK"}  
    3 - Na caixa de diálogo que aparece:  
        Selecione *Copy items if needed* para copiar o framework para o diretório do projeto  
        Marque a caixa de seleção para os destinos nos quais você planeja usar o framework  
![Detalhes]({{ site.url }}/img/fingerprintios3.png){: .left }{:title="Detalhes instalação SDK"}  
    4 - Clique em Finish  
    5 - Adicione os frameworks a seguir ao destino da aplicação no XCode:  
        - *ExternalAccessory.framework*. Se você verificar que o Wireless Accessory Configuration está ativado no Xcode 6 ou superior e não precisa, desativa e adicione novamente o ExternalAccessory.framework  
        - *CoreTelephony.framework*  
![Detalhes]({{ site.url }}/img/fingerprintios4.png){: .left }{:title="Detalhes instalação SDK"}  
    6 - Opcionalmente, adicione esses frameworks se o seu aplicativo fizer uso deles:  
        - *AdSupport.framework*. Se o seu aplicativo exibe anúncios  
        Obs.: Não incluir se o seu aplicativo não utilizar anúncios, pois a App Store rejeita aplicativos que incluem o framework mas não usam anúncios  
        - *CoreLocation.framework*. Se o seu aplicativo usa monitoramento local  
        Obs.: Não incluir, a menos que seu aplicativo solicite permissão de geolocalização do usuário  

* Usando a função +ioBegin  

A função *+ioBegin* coleta informações sobre o dispositivo e gera uma *caixa preta*. Esta *caixa preta* deverá ser enviada através do campo *CustomerData.BrowserFingerPrint* em conjunto com os outros dados para análise.  

* Sintaxe  

> NSSstring * bbox = [iovation ioBegin]  

* Valores de retorno  

> bbox - string que contem a *caixa preta*  

**IMPORTANTE!**  
A *caixa preta* que retornou de *+ioBegin* nunca deve estar vazio. Uma *caixa preta* vazia indica que a proteção oferecida pelo sistema pode ter sido comprometida.  

* Exemplo  

```html
#import "sampleViewController.h"
#import <iovation/iovation.h>
@implementation SampleViewController
@property (strong, nonatomic) UILabel *blackbox;
// Button press updates text field with blackbox value
- (IBAction)changeMessage:(id)sender
{
    self.blackbox.text = [iovation ioBegin];
}
@end

```

* Integrando com aplicativos Android  

Arquivos e requisitos de integração do Android  
![Detalhes]({{ site.url }}/img/fingerprintandroid.png){: .left }{:title="Detalhes integração Android"}  

**NOTA**  
Se as permissões listadas não são necessárias pelo aplicativo, os valores obtidos obtidos utilizando essas permissões serão ignorados. As permissões não são necessárias para obter uma *caixa preta*, mas ajudam a obter mais informações do dispositivo.  

A versão 1.2.0 do Iovation Mobile SDK para Android suporta versões do Android 2.1 ou superior.  

* Instalando o SDK no Android  

    1 - Baixe e descompacte o deviceprint-lib-1.2.0.aar  
    2 - Inicie o IDE de sua escolha  
    3 - No Eclipse e Maven, faça o deploy do arquivo de extensão *.aar* no repositório Maven local, usando o maven-deploy.  
        Mais detalhes em: [Maven Guide](http://maven.apache.org/guides/mini/guide-3rd-party-jars-local.html)
    4 - No Android Studio, selecione *File -> New Module*. Expande *More Modules* e escolha *Import existing .jar or .aar package*
    5 - Selecione o arquivo deviceprint-lib-1.2.0.aar, e clique em *Finish*
    6 - Certifique-se de que o device-lib é uma dependência de compilação no arquivo build.gradle  
![Detalhes]({{ site.url }}/img/fingerprintandroid1.png){: .left }{:title="Detalhes integração Android"}  

* Usando a função ioBegin  

A função *ioBegin* coleta informações sobre o dispositivo e gera uma *caixa preta*. Esta *caixa preta* deverá ser enviada através do campo *CustomerData.BrowserFingerPrint* em conjunto com os outros dados para análise.  

* Sintaxe  

> public static String ioBegin(Context context)

* Parâmetros

> context - uma instância da classe *android.content.Context* usado para acessar informações sobre o dispositivo

* Valores de retorno  

> string que contem a *caixa preta*  

**IMPORTANTE!**  
A *caixa preta* que retornou de *ioBegin* nunca deve estar vazio. Uma *caixa preta* vazia indica que contem apenas *0500* indica que a proteção oferecida pelo sistema pode ter sido comprometida.  

**IMPORTANTE!**  
O arquivo *device-lib-1.2.0.aar* deverá ser empacotado com o aplicativo.  

* Compilando o aplicativo de exemplo no Android Studio  

**IMPORTANTE!**  
Se a opção para executar o módulo não aparecer, selecione *File -> Project Structure* e abra o painel *Modules*. A partir disso, defina na lista a versão do Android SDK.

1 - Baixe e descompacte o deviceprint-lib-1.2.0.aar  
2 - No Android Studio, selecione *File -> Open* ou clique em *Open Project* através da opção *quick-start*  
3 - No diretório em que você descompactou o *deviceprint-lib-1.2.0.aar*, abra diretório *android-studio-sample-app* do aplicativo de exemplo  
4 - Abra o arquivo *DevicePrintSampleActivity*  
5 - Com algumas configurações, o Android Studio pode detectar um Android Framework no projeto e não configurá-lo. Neste caso, abra o *Event Log* e clique em *Configure*  
6 - Uma pop-up irá abrir para você selecionar o Android Framework. Clique em *OK* para corrigir os erros  
7 - No Android Studio, selecione *File -> New Module*. Expande *More Modules* e escolha *Import existing .jar or .aar package*  
8 - Selecione o arquivo deviceprint-lib-1.2.0.aar, e clique em *Finish*  
9 - Certifique-se de que o device-lib é uma dependência de compilação no arquivo build.gradle  
![Detalhes]({{ site.url }}/img/fingerprintandroid1.png){: .left }{:title="Detalhes integração Android"}  
10 - Abra a pasta DevicePrintSampleActivity  
11 - Na opção de navegação do projeto, abra *src/main/java/com/iovation/mobile/android/sample/DevicePrintSampleActivity.java*  
12 - Clique com o botão direito e selecione *Run DevicePrintSampleAct*  
13 - Selecione um dispositivo físico conectado ou um Android virtual para executar o aplicativo  
14 - O aplicativo irá compilar e executar  

* Exemplo  

O exemplo a seguir é simples, onde o mesmo possui um botão e ao clicar uma caixa de texto é preenchida com a *caixa preta*. Para obter um exemplo mais rico, consulte o aplicativo de exemplo do Android Studio incluído no SDK.  

```html
package com.iovation.mobile.android.sample;
import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.TextView;
import com.iovation.mobile.android.DevicePrint;
public class DevicePrintSampleActivity extends Activity
{
    /**
    * Called when the activity is first created.
    * @param savedInstanceState If the activity is being re-initialized after
    * previously being shut down then this Bundle contains the data it most
    * recently supplied in onSaveInstanceState(Bundle). <b>Note: Otherwise it is null.</b>
    */
    
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
    }
    
    public void printDevice(View target)
    {
        String bb = DevicePrint.ioBegin(getApplicationContext());
        TextView bbResult = (TextView) findViewById(R.id.bbResult);
        bbResult.setText(bb);
        bbResult.setVisibility(View.VISIBLE);
    }
}

```

## * Cybersource

Será necessário adicionar uma imagem de 1-pixel, que não é mostrada na tela, e 2 segmentos de código à tag *<body>* da sua página de checkout, se certificando que serão necessários de 10 segundos entre a execução do código e a submissão da página para o servidor.  

**IMPORTANTE!**  
Se os 3 segmentos de código não forem colocados na página de checkout, os resultados podem não ser precisos.  

**Colocando os segmentos de código e substituindo os valores das variáveis**  

Coloque os segmentos de código imediatamente acima da tag *</body>* para garantir que a página Web será renderizada corretamente. Nunca adicione os segmentos de código em elementos HTML visíveis. Os segmentos de código precisam ser carregados antes que o comprador finalize o pedido de compra, caso contrário um erro será gerado.  

Em cada segmento abaixo, substitua as variáveis com os valores referentes a loja e número do pedido.  

* Domain:  
    Testing - Use h.online-metrix.net, que é o DNS do servidor de fingerprint, como apresentado no exemplo de HTML abaixo.  
    Production - Altere o domínio para uma URL local, e configure seu servidor Web para redirecionar esta URL para h.online-metrix.net.  

**ProviderOrgId** - Para obter este valor, entre em contato com a Braspag.  
**ProviderMerchantId** - Para obter este valor, entre em contato com a Braspag.  
**ProviderSessionId** - Prencha este campo com o mesmo valor do campo **MerchantOrderId** que será enviado na requisição da análise de fraude.  

* PNG Image  

```html

<html>
<head></head>
<body>
    <form>
        <p style="background:url(https://h.online-metrix.net/fp/clear.png?org_id=ProviderOrgId&amp;session_id=ProviderMerchantIdProviderSessionId&amp;m=1)"></p>  
        <img src="https://h.online-metrix.net/fp/clear.png?org_id=ProviderOrgId&amp;session_id=ProviderMerchantIdProviderSessionId&amp;m=2" alt="">  
    </form>
</body>
</html>

```

* Flash Code  

```html

<html>
<head></head>
<body>
    <form>
        <object type="application/x-shockwave-flash" data="https://h.online-metrix.net/fp/fp.swf?org_id=ProviderOrgId&amp;session_id=ProviderMerchantIdProviderSessionId" width="1" height="1" id="thm_fp">
            <param name="movie" value="https://h.online-metrix.net/fp/fp.swf?org_id=ProviderOrgId&amp;session_id=ProviderMerchantIdProviderSessionId" />
            <div></div>
        </object>
    </form>
</body>
</html>

```

* Javascript Code

```html

<html>
<head></head>
<script src="https://h.online-metrix.net/fp/check.js?org_id=ProviderOrgId&amp;session_id=ProviderMerchantIdProviderSessionId" type="text/javascript"></script>
<body></body>
</html>

```

**IMPORTANTE!**  
Certifique-se de copiar todos os dados corretamente e de ter substituído as variáveis corretamente pelos respectivos valores.  

**Configurando seu Servidor Web**  

Na seção *Colocando os segmentos de código e substituindo os valores das variáveis (Domain)*, todos os objetos se referem a h.online-metrix.net, que é o DNS do servidor de fingerprint. Quando você estiver pronto para produção, você deve alterar o nome do servidor para uma URL local, e configurar no seu servidor Web um redirecionamento de URL para h.online-metrix.net.  

**IMPORTANTE!**  
Se você não completar essa seção, você não receberá resultados corretos, e o domínio (URL) do fornecedor de fingerprint ficará visível, sendo mais provável que seu consumidor o bloqueie.  