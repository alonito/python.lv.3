import telebot 
from config import token

from logic import Pokemon

bot = telebot.TeleBot("8402641074:AAFbuEw_tE2NDrYXAvClsFk7DCyHbJQ7ub8") 

@bot.message_handler(commands=['go'])
def go(message):
    if message.from_user.username not in Pokemon.pokemons.keys():
        pokemon = Pokemon(message.from_user.username)
        bot.send_message(message.chat.id, pokemon.info())
        bot.send_photo(message.chat.id, pokemon.show_img())
    else:
        bot.reply_to(message, "Ты уже создал себе покемона")


bot.infinity_polling(none_stop=True)


if message.from_user.username in Pokemon.pokemons.keys():

    pok = Pokemon.pokemons[message.from_user.username]


@bot.message_handler(commands=['help', 'start'])
def send_welcome(message):
    bot.reply_to(message, """\
 Hi there, I am EchoBot.
 I am here to echo your kind words back to you. Just say anything nice and I'll say the exact same thing to you!\
 """)
    


bot.infinity_polling()



def attack_handler(message):
    attacker_username = message.from_user.username
    defender_username = message.reply_to_message.from_user.username

    if not attacker_username or not defender_username:
        bot.reply_to(message, "Both users must have a username.")
        return

    if attacker_username not in pokemons:
        bot.reply_to(message, f"@{attacker_username} has no pokemon.")
        return

    if defender_username not in pokemons:
        bot.reply_to(message, f"@{defender_username} has no pokemon.")
        return

    attacker_pokemon = pokemons[attacker_username]
    defender_pokemon = pokemons[defender_username]

    if hasattr(attacker_pokemon, "fight"):
        result = attacker_pokemon.fight(defender_pokemon)
        bot.reply_to(message, result if isinstance(result, str) else "Battle started!")
    else:
        bot.reply_to(
            message,
            f" Battle started!\n@{attacker_username} ({attacker_pokemon.name}) vs @{defender_username} ({defender_pokemon.name})"
        )        
