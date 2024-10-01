README
Português
Descrição do Projeto
Este projeto tem como objetivo realizar a manipulação, interpolação e correção de dados magnéticos base e itinerante, utilizando dados de séries temporais e cálculos de incrementos/decrementos magnéticos. Ele processa os dados de duas fontes (base e itinerante), compara os valores e aplica correções aos dados itinerantes com base nas variações observadas na estação base.

Instalação
Clone o repositório para o seu ambiente local:

bash
Copy code
git clone https://github.com/seu-usuario/seu-repositorio.git
Certifique-se de ter o Python 3.x e as bibliotecas necessárias instaladas. Instale as dependências com:

bash
Copy code
pip install -r requirements.txt
Utilização
Importação dos Dados

Os dados são importados de arquivos .txt contendo colunas de informações magnéticas. Esses arquivos devem estar formatados corretamente com delimitadores de espaço.
python
Copy code
mag_base = pd.read_csv('base_0309.txt', delim_whitespace=True)
mag_itinerante = pd.read_csv('itinerante_0309.txt', delim_whitespace=True)
Manipulação dos Dados

As colunas desnecessárias são removidas, e os DataFrames são limpos para manter apenas as colunas relevantes para a análise.
python
Copy code
mag_base = mag_base.drop(['del1', 'del2','del3','Number_Station'], axis=1)
mag_itinerante = mag_itinerante.drop(['del1', 'del2','Number_Station'], axis=1)
Interpolação de Dados

A interpolação é aplicada aos dados da estação base para preencher valores de tempo que possam estar faltando ou espaçados de forma irregular.
python
Copy code
interval = np.arange(81434.0, 145134.0, 1)
expanded_df = pd.DataFrame({'Hour': interval})
mag_base_df = pd.merge(expanded_df, mag_base_df, on='Hour', how='left')
mag_base_df = mag_base_df.interpolate(method='spline', order=1)
Correção dos Dados Itinerantes

Após a interpolação, os valores de "H_base" são extraídos e comparados com os valores itinerantes. Incrementos e decrementos são calculados para ajustar o campo magnético da estação itinerante de acordo com as variações na estação base.
python
Copy code
delta = []
for i in H_base:
    delta.append((i-H_base[0]))
mag_itinerante_df['Delta'] = delta
mag_itinerante_df['H_final'] = mag_itinerante_df['H'] + mag_itinerante_df['Delta']
Visualização dos Dados

Gráficos e tabelas são gerados para visualizar as primeiras 20 linhas dos dados da estação base e itinerante, bem como os resultados da correção aplicada.
python
Copy code
mag_itinerante_df.plot(x='Hour', y=['H_final', 'H_base', 'H'], kind='line')
plt.title('Campo Magnético da Itinerante')
plt.xlabel('Hour (s)')
plt.ylabel('Campo Magnético (nT)')
plt.show()
Resultados
O código gera um novo DataFrame para os dados itinerantes, contendo a coluna "H_final", que é o campo magnético corrigido. Essa correção se baseia nos incrementos e decrementos derivados da comparação com a estação base.
O resultado visual é um gráfico que mostra a comparação entre o campo magnético bruto da estação itinerante, o campo magnético interpolado da base, e o campo magnético final corrigido para a estação itinerante.
A partir dessa visualização, é possível identificar as variações no campo magnético itinerante que podem ser atribuídas a influências externas ou correções necessárias devido ao comportamento da estação base.
Esse processo permite que os usuários compreendam melhor como as correções foram aplicadas e visualizem os resultados das manipulações e interpolações.

Conclusão dos Resultados Finais
Os resultados finais mostram a adequação dos dados itinerantes após a correção, com uma melhor correspondência em relação aos dados da estação base. Através da interpolação dos dados base e da aplicação dos incrementos e decrementos, o campo magnético itinerante foi ajustado de forma a refletir melhor as condições magnéticas reais.

Esse processo pode ser utilizado em análises subsequentes de dados magnéticos, permitindo uma correção mais precisa e confiável. Os gráficos gerados ajudam a identificar qualquer discrepância que possa existir entre os dados brutos e os corrigidos, oferecendo uma visão clara das correções aplicadas.

English
Project Description
This project aims to manipulate, interpolate, and correct base and itinerant magnetic data using time series data and magnetic increment/decrement calculations. It processes data from two sources (base and itinerant), compares the values, and applies corrections to the itinerant data based on the variations observed in the base station.

Installation
Clone the repository to your local environment:

bash
Copy code
git clone https://github.com/your-username/your-repo.git
Make sure you have Python 3.x and the necessary libraries installed. Install the dependencies with:

bash
Copy code
pip install -r requirements.txt
Usage
Data Import

The data is imported from .txt files containing magnetic information columns. These files should be properly formatted with whitespace delimiters.
python
Copy code
mag_base = pd.read_csv('base_0309.txt', delim_whitespace=True)
mag_itinerante = pd.read_csv('itinerante_0309.txt', delim_whitespace=True)
Data Manipulation

Unnecessary columns are removed, and the DataFrames are cleaned to retain only relevant columns for analysis.
python
Copy code
mag_base = mag_base.drop(['del1', 'del2','del3','Number_Station'], axis=1)
mag_itinerante = mag_itinerante.drop(['del1', 'del2','Number_Station'], axis=1)
Data Interpolation

Interpolation is applied to the base station data to fill in missing or irregularly spaced time values.
python
Copy code
interval = np.arange(81434.0, 145134.0, 1)
expanded_df = pd.DataFrame({'Hour': interval})
mag_base_df = pd.merge(expanded_df, mag_base_df, on='Hour', how='left')
mag_base_df = mag_base_df.interpolate(method='spline', order=1)
Itinerant Data Correction

After interpolation, the "H_base" values are extracted and compared with the itinerant values. Increments and decrements are calculated to adjust the itinerant magnetic field according to the variations in the base station.
python
Copy code
delta = []
for i in H_base:
    delta.append((i-H_base[0]))
mag_itinerante_df['Delta'] = delta
mag_itinerante_df['H_final'] = mag_itinerante_df['H'] + mag_itinerante_df['Delta']
Data Visualization

Graphs and tables are generated to visualize the first 20 rows of the base and itinerant station data, as well as the results of the applied correction.
python
Copy code
mag_itinerante_df.plot(x='Hour', y=['H_final', 'H_base', 'H'], kind='line')
plt.title('Itinerant Magnetic Field')
plt.xlabel('Hour (s)')
plt.ylabel('Magnetic Field (nT)')
plt.show()
Results
The code generates a new DataFrame for the itinerant data, containing the column "H_final," which is the corrected magnetic field. This correction is based on the increments and decrements derived from comparing with the base station.
The visual output is a graph showing the comparison between the raw itinerant magnetic field, the interpolated base magnetic field, and the final corrected itinerant magnetic field.
From this visualization, users can identify variations in the itinerant magnetic field that can be attributed to external influences or necessary corrections based on the behavior of the base station.
Conclusion of Final Results
The final results demonstrate the adjustment of the itinerant data after the correction, showing better alignment with the base station data. Through interpolation of the base data and the application of increments and decrements, the itinerant magnetic field has been adjusted to better reflect real magnetic conditions.

This process can be used in subsequent analyses of magnetic data, allowing for more precise and reliable correction. The generated graphs help identify any discrepancies between the raw and corrected data, providing a clear view of the applied corrections.



