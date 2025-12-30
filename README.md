# Realtime Chat Application

## Student Information
*Name:* NUR WAFA NABILA BINTI ZIN ADLI
*Student ID:* 2025248132
*Group:* CS2703C
*Lecturer:* MUHAMMAD ATIF BIN RAMLAN

## Project Background
In today's digital world, real-time communication has been widely spread across the world, where it has become a fundamental necessity for an individual to connect and interact. The shift from instant messaging to real-time chat has transformed digital interactions into fluidity. Its objective is to create a functional real-time chat application that allows users to log in and exchange messages instantly.

## Discussion
The main purpose of this lab activity is to teach students to build a real-time chat application and let students explore the function of Angular and Supabase and how to store data with secure security. Real-time chat is an application that allows users to send messages to each other. Examples of the application are WhatsApp and Telegram. For this lab activity, the tools that I use are Visual Studio Code to build this real-time chat application, Angular and Supabase as the storage for the database. The project helps me to understand and gain knowledge about real-time systems and how to connect it with Angular and Supabase. It is my first time using Supabase, I browse and search a lot of Youtube videos to learn about Supabase and the function of it. In the Supabase, I learn about authentication, database, storage and API. Moreover, I learn to create a database and create tables and write SQL in Supabase. It is a fun experience as a mobile computing student as I never got this chance to learn about back end development before.

While trying to complete this lab activity, I faced some difficulties setting up Supabase. Some of the difficulties are that I made a mistake while setting up user authentication in the sign in and providers section. The issue causes the user to not be able to sign in using a Google account. I spent a lot of time trying to resolve the problems that I am facing with user authentication which is at configuration where I need to create policy. After watching countless tutorial videos on Youtube and testing the application multiple times, finally I was able to solve the issues.  Moreover, the message does not appear after the send button is clicked, the profile picture of the user is not showing and the dropdown button for delete is not working which means the messages that user sent cannot be deleted. To fix this problem, I check the code in Visual Studio Code to see if there is anything that I forgot to insert. To conclude, this process taught me the importance of setting up the Supabase and checking the code in Visual Studio Code, and one mistake can cause an effect on other things.

After completing the real-time chat, I use Bootstrap to design the interface of the application. I design the buttons, chat box, background, input box, and put suitable colors for chat applications to ensure a consistent look in the css file. I divide the function and style file so it is easy for me to fix if there are any errors. Then, I am making sure the application is web-friendly where users can minimize and maximize the app freely without making the page. I do some research too on how to design the interface. Some of the websites that help me a lot are W3School, MDN Web Docs and GeeksforGeeks. With the help of these websites, I was able to implement it in designing the interface.

In conclusion, I have successfully completed this lab activity. All the challenges I faced taught me to be patient and always not give up and it is normal to get errors. All the skills that I have gained throughout this lab activity will help in future projects especially for my final year projects and during my internship. This lab activity inspired me to build other real-life systems other than real-time chat applications. Moreover, I can put this project in my portfolio to strengthen my skill.


<!-- ## Database Table Schema -->
## users table

* id (uuid)
* full_name (text)
* avatar_url (text)

## Creating a users table

```sql
CREATE TABLE public.users (
   id uuid not null references auth.users on delete cascade,
   full_name text NULL,
   avatar_url text NULL,
   primary key (id)
);
```

## Enable Row Level Security

```sql
ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;
```

## Permit Users Access Their Profile

```sql
CREATE POLICY "Permit Users to Access Their Profile"
  ON public.users
  FOR SELECT
  USING ( auth.uid() = id );
```

## Permit Users to Update Their Profile

```sql
CREATE POLICY "Permit Users to Update Their Profile"
  ON public.users
  FOR UPDATE
  USING ( auth.uid() = id );
```

## Supabase Functions

```sql
CREATE
OR REPLACE FUNCTION public.user_profile() RETURNS TRIGGER AS $$ BEGIN INSERT INTO public.users (id, full_name,avatar_url)
VALUES
  (
    NEW.id,
    NEW.raw_user_meta_data ->> 'full_name'::TEXT,
    NEW.raw_user_meta_data ->> 'avatar_url'::TEXT,
  );
RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;
```

## Supabase Trigger

```sql
  CREATE TRIGGER
  create_user_trigger
  AFTER INSERT ON auth.users
  FOR EACH ROW
  EXECUTE PROCEDURE
    public.user_profile();
```

## Chat_Messages table (Real Time)

* id (uuid)
* Created At (date)
* text (text)
* editable (boolean)
* sender (uuid)

