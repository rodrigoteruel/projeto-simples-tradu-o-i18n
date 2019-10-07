## fonte da pesquisa 
https://www.codeandweb.com/babeledit/tutorials/how-to-translate-your-angular8-app-with-ngx-translate


## instalação das bibliotecas para tradução i18n
npm install @ngx-translate/core @ngx-translate/http-loader rxjs --save


## Iniciar a tradução TranslateModule no app.module.ts:

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

// import ngx-translate and the http loader
import {TranslateLoader, TranslateModule} from '@ngx-translate/core';
import {TranslateHttpLoader} from '@ngx-translate/http-loader';
import {HttpClient, HttpClientModule} from '@angular/common/http';

@NgModule({
    declarations: [
        AppComponent
    ],
    imports: [
        BrowserModule,

        // ngx-translate and the loader module
        HttpClientModule,
        TranslateModule.forRoot({
            loader: {
                provide: TranslateLoader,
                useFactory: HttpLoaderFactory,
                deps: [HttpClient]
            }
        })
    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule { }

// required for AOT compilation
export function HttpLoaderFactory(http: HttpClient) {
    return new TranslateHttpLoader(http);
}

## Agora alterne to app.component.ts:

import {Component} from '@angular/core';
import {TranslateService} from '@ngx-translate/core';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.scss']
})
export class AppComponent {
    constructor(private translate: TranslateService) {
        translate.setDefaultLang('en');
    }
}

## Criar o arquivo JSON para tradução inglês em: assets/i18n/en.json.

ngx-translate can read 2 JSON formats:

{
  "demo.title": "Translation demo",
  "demo.text": "This is a simple demonstration app for ngx-translate"
}

## Usando tradução 
title and text are identifiers ngx-translate uses to find the translation strings. Now edit the app.component.html

You have two choices when it comes to adding translations:

translation pipe — {{'id' | translate}}
translation directive — <element [translate]="'id'"></element>
Both options lead to the same result — it's just a matter of personal tast which one you want to use.

<div>
    <!-- translation: translation pipe -->
    <h1>{{ 'demo.title' | translate }}</h1>

    <!-- translation: directive (key as attribute)-->
    <p [translate]="'demo.text'"></p>

    <!-- translation: directive (key as content of element) -->
    <p translate>demo.text</p>
</div>
Translations with parameters
ngx-translate also supports parameters in translations. They are passed as an object, the keys can be used in the translation strings.

<!-- translation: translation pipe -->
<p>{{ 'demo.greeting' | translate:{'name':'Andreas'} }}</p>

<!-- translation: directive (key as attribute) -->
<p [translate]="'demo.greeting'" [translateParams]="{name: 'Andreas'}"></p>

<!-- translation: directive (key as content of element)-->
<p translate [translateParams]="{name: 'Andreas'}">demo.greeting</p>
And the extended translations file:

{
  "demo": {
    …
    "greeting": "Hello {{name}}!"
  }
}


## tradução com parametros

<div>
  <!-- translation: translation pipe -->
  <h1>{{ 'demo.title' | translate }}</h1>

  <!-- translation: directive (key as attribute)-->
  <p [translate]="'demo.text'"></p>

  <!-- translation: directive (key as content of element) -->
  <p translate>demo.text</p>
  
  <!-- translation: translation pipe -->
  <p>{{ 'demo.greeting' | translate:{'name':'Andreas'} }}</p>

  <!-- translation: directive (key as attribute) -->
  <p [translate]="'demo.greeting'" [translateParams]="{name: 'Andreas'}"></p>

  <!-- translation: directive (key as content of element)-->
  <p translate [translateParams]="{name: 'Andreas'}">demo.greeting</p>
</div>