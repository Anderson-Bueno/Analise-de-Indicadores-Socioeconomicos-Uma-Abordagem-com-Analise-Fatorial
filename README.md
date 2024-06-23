Análise de Indicadores Socioeconômicos: Uma Abordagem com Análise Fatorial

---

Introdução


Este caso destaca a aplicação de técnicas avançadas de análise de dados para entender a relação entre diferentes indicadores socioeconômicos, como escolaridade, PIB per capita e violência. Utilizando a Análise Fatorial por Componentes Principais (PCA), este estudo proporciona insights valiosos para a elaboração de políticas públicas e planejamento estratégico.

---

Desafio


O desafio principal era identificar e analisar a relação entre diferentes indicadores socioeconômicos em diversos países, ao longo de dois anos, para fornecer uma base sólida para tomada de decisões informadas em políticas públicas. A complexidade e a inter-relação dos dados exigiam uma abordagem robusta de análise que pudesse reduzir a dimensionalidade dos dados, mantendo a maior quantidade de informação possível.

---

Solução


A solução foi implementar a Análise Fatorial por Componentes Principais (PCA) para reduzir a dimensionalidade dos dados e identificar os principais fatores que explicam a variabilidade nos indicadores socioeconômicos. Ferramentas de visualização e análise de correlação foram utilizadas para interpretar os resultados e facilitar a compreensão das relações entre as variáveis.

---

Implementação

1. Instalação e Carregamento dos Pacotes:

   pacotes <- c("plotly", "tidyverse", "ggrepel", "knitr", "kableExtra", "reshape2", "PerformanceAnalytics", "psych", "ltm", "Hmisc", "readxl")
   if(sum(as.numeric(!pacotes %in% installed.packages())) != 0){
     instalador <- pacotes[!pacotes %in% installed.packages()]
     for(i in 1:length(instalador)) {
       install.packages(instalador, dependencies = T)
       break()}
     sapply(pacotes, require, character = T) 
   } else {
     sapply(pacotes, require, character = T) 
   }
   

2. Carregamento e Visualização dos Dados:
   
   base_indicador <- read_xlsx("Indicador.xlsx")
   base_indicador %>%
     kable() %>%
     kable_styling(bootstrap_options = "striped", full_width = FALSE, font_size = 20)
   

3. Estatísticas Descritivas:
   
   summary(base_indicador)
   

4. Análise de Correlação:
   
   rho <- rcorr(as.matrix(base_indicador[,2:5]), type="pearson")
   rho$r  # Matriz de correlações
   rho$P  # Matriz com p-valor dos coeficientes
   

5. Visualização das Correlações:
   
   ggplotly(
     base_indicador[,2:5] %>%
       cor() %>%
       melt() %>%
       rename(Correlação = value) %>%
       ggplot() +
       geom_tile(aes(x = Var1, y = Var2, fill = Correlação)) +
       geom_text(aes(x = Var1, y = Var2, label = format(Correlação, digits = 1)),
                 size = 5) +
       scale_fill_viridis_b() +
       labs(x = NULL, y = NULL) +
       theme_bw()
   )
   

6. Análise Fatorial por Componentes Principais (PCA):
   
   fatorial <- principal(base_indicador[, 2:5], nfactors = length(base_indicador[, 2:5]), rotate = "none", scores = TRUE)
   fatorial
   eigenvalues <- round(fatorial$values, 5)
   eigenvalues
   round(sum(eigenvalues), 2)
   

7. Visualização das Variâncias Compartilhadas:
   
   variancia_compartilhada <- as.data.frame(fatorial$Vaccounted) %>% slice(1:3)
   rownames(variancia_compartilhada) <- c("Autovalores", "Prop. da Variância", "Prop. da Variância Acumulada")
   round(variancia_compartilhada, 3) %>%
     kable() %>%
     kable_styling(bootstrap_options = "striped", full_width = FALSE, font_size = 20)
   

8. Cálculo e Visualização das Cargas Fatoriais:
   
   cargas_fatoriais <- as.data.frame(unclass(fatorial$loadings))
   round(cargas_fatoriais, 3) %>%
     kable() %>%
     kable_styling(bootstrap_options = "striped", full_width = FALSE, font_size = 20)
   

9. Cálculo dos Scores Fatoriais:
   
   scores_fatoriais <- as.data.frame(fatorial$weights)
   round(scores_fatoriais, 3) %>%
     kable() %>%
     kable_styling(bootstrap_options = "striped", full_width = FALSE, font_size = 20)
   fatores <- as.data.frame(fatorial$scores)
   

10. Elaboração da PCA para Anos Separados:

    base_indicador <- bind_cols(base_indicador, "fator_ano1" = fatores$PC1)
    fatorial2 <- principal(base_indicador[, 6:9], nfactors = k, rotate = "none", scores = TRUE)
    fatores2 <- as.data.frame(fatorial2$scores)
    base_indicador <- bind_cols(base_indicador, "fator_ano2" = fatores2$PC1)
  

---

Resultados


A Análise Fatorial por Componentes Principais permitiu a redução da dimensionalidade dos dados e a identificação de fatores que explicam a variabilidade nos indicadores socioeconômicos. Os resultados foram visualizados através de gráficos de correlação, mapas de calor e gráficos de dispersão, fornecendo uma visão clara das relações entre as variáveis e facilitando a interpretação dos dados.

---

Conclusão


O uso da Análise Fatorial por Componentes Principais revelou-se uma ferramenta poderosa para a análise de indicadores socioeconômicos. Este caso de sucesso demonstra como a aplicação de técnicas avançadas de análise de dados pode fornecer insights valiosos e apoiar a tomada de decisões informadas em políticas públicas e planejamento estratégico.

---


"A aplicação da Análise Fatorial nos permitiu compreender melhor a complexa relação entre diferentes indicadores socioeconômicos. Isso nos forneceu uma base sólida para a elaboração de políticas públicas eficazes." – Economista Chefe

---


Interessado em saber mais sobre como a análise de dados pode transformar seu negócio ou pesquisa? Entre em contato conosco ou visite nosso site para mais informações.

