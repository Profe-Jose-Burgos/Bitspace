# Bitspace
## Integrantes:
### Shirley Batista
### Cristel Mariel Ortiz
### Angy Santana
### Maria de Los Angeles Gonzales

import random
import re
def get_response(user_input):
  split_message=re.split(r'\s|[,:;.?!-_]\s*',user_input.lower())
  response=check_all_message(split_message)
  return response

def message_propability(user_message, recognized_word, single_response=False, required_word=[]):
  message_certainty=0
  has_required_words= True 
  for word in user_message:
    if word in recognized_word:
      message_certainty +=1

    porcentaje=float(message_certainty)/float(len(recognized_word))

    for word in required_word:
      if word not in user_message:
        has_required_words= False
        break
    if has_required_words or single_response:
      return int(porcentaje*100)
    else:
      return 0

 

def check_all_message(message):
  highest_prob={}

  def response(bot_response, list_of_words, single_response=False, required_word=[]):
    nonlocal highest_prob
    highest_prob[bot_response]=message_propability(message,list_of_words,single_response,required_word)
  
  #print("Hola, soy Luna tu asistente virtual") #me repite, arreglar. Debe decirlo una sola vez
  response('Bienvenido Cliente, ¿en que podemos ayudarle?',['hola','consulta','saludos','buenas','una consulta','una pregunta'],single_response=True)
  response('Estamos ubicados en Calle5ta. Rio Abajo, Panamá, República de Panamá, Galera Empresas Carbone',['ubicados','direccion','donde','ubicacion','encontrarlo'],single_response=True)
  response('Nuestro horario es: \n|Lunes a Jueves de 8:00 a.m. a 6:00 p.m.|\n|Viernes de 8:00 a.m. a 5:00 p.m.|\n|Sábados y Domingos CERRADOS|',['horario', 'horario', 'cierre','abierto'],single_response=True,required_word=['abierto','abren']) #arreglar, no me entiende el abren... 
  response('Tel.(+507)391-6309/391-6313\nContacto: ricardo@carbone.com.pa') #
  response('Estamos a la orden',['gracias','te lo agradezco','thanks'],single_response=True)

  best_match=max(highest_prob,key=highest_prob.get)
  #print(highest_prob)
  return unknown() if highest_prob[best_match]<1 else best_match

def unknown():
  response=['¿puedes decirlo de nuevo?','no estoy seguro de lo que necesitas','no logro entenderte'][random.randrange(3)]
  return response

while True:
  print("Luna: "+get_response(input('Usuario: ')))
