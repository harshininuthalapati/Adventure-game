def show_instructions():
    print('\nWelcome to the text based game: ')
    print("Escape From the House")
    print(
        'The main objective of this game is to find 8 objects\nto help you defeat the Enemy.')
    print('\nMove commands you can use are: West, East, North, South')
    print('Additional helpful commands are: yes, no, quit')


def main():
    rooms = {

        'Attic': {'name': 'Attic', 'North': 'Upstairs Hallway'},  
        'Upstairs Hallway': {'name': 'Upstairs Hallway', 'South': 'Attic', 'North': 'Upstairs Bedroom',
                             'East': 'Downstairs Hallway', 'item': 'bandages', },
        'Upstairs Bedroom': {'name': 'Upstairs Bedroom', 'South': 'Upstairs Hallway', 'item': 'lighter',
                             'room_extras': 'closet'},
        'Downstairs Hallway': {'name': 'Downstairs Hallway', 'West': 'Upstairs Hallway', 'North': 'Bathroom',
                               'East': 'Dining Room', 'South': 'Garage', 'item': 'gun'},
        'Bathroom': {'name': 'Bathroom', 'South': 'Downstairs Hallway', 'East': 'Bedroom', 'item': 'painkillers'},
        'Bedroom': {'name': 'Bedroom', 'West': 'Bathroom', 'South': 'Kitchen', 'item': 'Bullets',
                    'room_extras': 'wall safe'},
        'Kitchen': {'name': 'Kitchen', 'North': 'Bedroom', 'South': 'Dining Room', 'item': 'fire extinguisher'},
        'Dining Room': {'name': 'Dining Room', 'North': 'Kitchen', 'West': 'Downstairs Hallway', 'item': 'samosa',
                        'room_extras': 'book shelf'},
        'Garage': {'name': 'Garage', 'North': 'Downstairs Hallway', 'East': 'Basement', 'item': 'petrol',
                   'room_extras': 'car'},
        'Basement': {'name': 'Basement', 'West': 'Garage', 'item': 'Gona Ganna Reddy'}  
    }
    directions = ['North', 'South', 'East', 'West']
    termination = ['Quit', 'quit', 'QUIT']
    commands = ['Yes', 'No']
    current_room = rooms['Attic']
    inventory = []

    show_instructions()
    print('\nYou start in the Attic, good luck!')
    while True:
        player_input = input('\nWhat is your move? ')  

        print('Your Inventory\n', inventory)  
        if player_input.capitalize() in directions:  
            if player_input.capitalize() in current_room:  
                current_room = rooms[current_room[player_input.capitalize()]] 
                print('You are in the {}.'.format(current_room['name']))
                if current_room['name'] == 'Basement' and len(inventory) == 8:
                    print('All the necessary items were collected!')
                    print('You defeated Gona Ganna Reddy\nCongratulations, you escaped!')
                    break
                elif current_room['name'] == 'Basement' and len(inventory) != 8:  
                    print(
                        'You did not gather all the necessary items,\nGona Ganna Reddy killed you!\nGood luck next time!')
                    break
                elif 'room_extras' in current_room:  
                    print('There is a {}.'.format(current_room['room_extras']))
                    input_question = input(
                        'Would you like to check what is inside? (Yes or No) ').capitalize()  
                    while input_question.capitalize() != 'Yes' or input_question.capitalize() != 'No': 
                        if input_question in commands:  
                            if input_question.capitalize() == 'Yes':  
                                print('You see {}.'.format(current_room['item']))
                                add_item = input('Would you like to take the item? (Yes or No) ').capitalize()  
                                while add_item.capitalize() != 'Yes' or add_item.capitalize() != 'No':  
                                    if add_item.capitalize() == 'Yes':  
                                        print('{} added to your inventory.'.format(current_room['item']))
                                        inventory.append(current_room['item'])
                                        del current_room['room_extras']  
                                        del current_room['item']  
                                        break
                                    elif add_item.capitalize() == 'No': 
                                        print('Continue with your exploration!')
                                        del current_room['room_extras']   
                                        break
                                    else:
                                        add_item = input('Would you like to take the item? (Yes or No) ').capitalize()  
                                break
                            elif input_question.capitalize() == 'No':  # 
                                print('Continue on with exploration of the house.')
                                break
                        else: 
                            print('You must input Yes/ No! Try again!')
                            input_question = input('Would you like to check what is inside? (Yes or No) ').capitalize()
                elif 'item' not in current_room.keys():  
                    print('There is no item here, keep on looking.')
                elif current_room['item'] in current_room.values():  
                    print('You see {}'.format(current_room['item']))
                    add_item = input('Would you like to take the item? (Yes or No) ').capitalize() 
                    while add_item.capitalize() != 'Yes' or add_item.capitalize() != 'No':  
                        if add_item.capitalize() == 'Yes':  
                            print('{} added to your inventory.'.format(current_room['item']))
                            inventory.append(current_room['item'])  
                            del current_room['item']  
                            break
                        elif add_item.capitalize() == 'No':  
                            print('Continue with your exploration!')
                            break
                        else:
                            add_item = input('Invalid move type Yes/ No! ').capitalize() 
            elif player_input.capitalize() not in current_room:  
                print('You cannot go that way, try again!')
        elif player_input in termination:  
            print('Sorry to see you go. Have a nice day!')
            break
        elif player_input.capitalize() not in directions:  
            print('Invalid move, try again!')

main()

