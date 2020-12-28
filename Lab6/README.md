# Refactoring

## Laboratorio 6

Refactorizar el proyecto de software [WACeL-Java](https://github.com/edgarsc22/WACeL-Java) las actividades que se realizaron son las siguientes:


#	Refactoring

##	Aplicar prácticas de "Clean Code"

- Los nombres para variables, funciones, clases, parametros, metodos, entre otros son precisos y representan su idea central.

- Las funciones dentro del codigo en algunos casos son extensas y/o poseen dentro de ellas codigo repetido.

- Los comentarios fueron utiles para entender el codigo, pero tambien existe parte del codigo comentado en lugar de ser eliminado.

##	Aplicar estrategias de "refactoring"

Existen distintos patrones de diseno que podrian mejorar el codigo que se detallara a continuacion:

### Factory

Es un patrón de diseño creacional y que sirve para construir una jerarquía de clases. Existen varias partes del codigo donde se encunetra de la siguiente manera. El patron de diseno encalsularia las sentencias, para paserle mediante paramentros segun la condicional indique.

``` java
//Condicional {

	Defect defect = new Defect(); 
	defect.setQualityProperty(QualityPropertyEnum.ATOMICITY.getQualityProperty());
	defect.setScenarioId(structuredScenario.getId());
	defect.setScenarioElement(ScenarioElement.TITLE.getScenarioElement());

	// Las siguientes lineas varian segun la condicional pero estan presentes

	defect.setIndicator(..................................);

	defect.setIndicator(..................................);
	defect.setIndicator(..................................);


	defect.setDefectCategory(.......................);
	defect.setDefectCategory(........................);	


	defect.setFixRecomendation(................);

	// Al final termina en todas las condicionales de la manera
	defects.add(defect)	;
}

```


De la misma manera para el codigo de ScenarioServicempl que posee ifs en cascada con bucles que se repiten segun su condicional
``` java
if(structuredScenario.getEpisodes() != null && !structuredScenario.getEpisodes().isEmpty()) {								
	for(StructuredEpisode episode : structuredScenario.getEpisodes()) {
		if(episode.getPreConditions() != null && !episode.getPreConditions().isEmpty()) {
			for(String preCondition : episode.getPreConditions()) {
				//Episodes pre-condition (Si) vs Context post-condition (Sj)
				if(relatedScenario.getContext().getPostConditions() != null && !relatedScenario.getContext().getPostConditions().isEmpty()) {
					if(relatedScenario.getContext().getPostConditions().contains(preCondition)) {
						preCondSiPostCondSj = true;
						break;
					}
				}
				//Episodes pre-condition (Si) vs Episodes post-condition (Sj)
				if(relatedScenario.getEpisodes() != null && !relatedScenario.getEpisodes().isEmpty()) {
					for(StructuredEpisode otherEpisode : relatedScenario.getEpisodes()) {
						if(otherEpisode.getPostConditions() != null && !otherEpisode.getPostConditions().isEmpty())
							if(otherEpisode.getPostConditions().contains(preCondition)) {
								preCondSiPostCondSj = true;
								break;
							}										
					}
				}								
				if(preCondSiPostCondSj)
					break;
			}
		}
		if(preCondSiPostCondSj)
			break;
	}
}

```
