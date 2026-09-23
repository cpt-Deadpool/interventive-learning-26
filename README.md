# Interventive Learning Demo Setup Guide

## Part 1 — One-Time Installations

These only need to be installed once. If you have already installed them, skip to Part 2.

### Install Docker Desktop

Docker is the program that runs the database the demo needs to function.

1. Go to: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. Click **Download Docker Desktop**
3. Choose Windows (or Mac if you are on a Mac)
4. Once the file downloads, open it and follow the installation steps:
   * Click Next
   * Agree to the terms
   * Click Install
5. When installation finishes, restart your computer
6. After restarting, open Docker Desktop from your Start menu or desktop shortcut
7. Wait until you see “Docker Desktop is running” in the bottom-left corner of the Docker window

This may take a minute or two.

---

### Install Python

Python is the programming language the demo is written in.

1. Go to: [https://www.python.org/downloads/](https://www.python.org/downloads/)
2. Click the yellow **Download Python** button at the top
3. Once the file downloads, open it
4. On the first screen of the installer, check the box that says:
Add Python to PATH

This step is critical and should not be skipped.

5. Click **Install Now**
6. Wait for installation to finish, then click **Close**

---

# Part 2 — Open a Terminal (Command Prompt)

A terminal is a text-based window where you type commands.

## How to open it

1. Press the Windows key on your keyboard
2. Type:
cmd

3. Press Enter

A black window will open. This is your terminal. Keep it open for the remaining steps.

---

# Part 3 — Navigate to the Demo Folder

You need to tell the terminal where the demo files are saved on your computer.

In the terminal, type:
cd C:\Users\YourName\Downloads\interventive-learning

Replace `YourName` with your actual Windows username and update the path if the folder is saved somewhere else.

Press Enter.

## How to find the correct path

1. Open File Explorer
2. Find the `interventive-learning` folder
3. Click the address bar at the top of File Explorer
4. Copy the full path
5. Paste it after `cd` in the terminal

Example:
cd C:\Users\Riya\Downloads\interventive-learning

---

# Part 4 — Start the Database (First Time Only)

This creates the database that stores all the reading content. You only need to do this once.

Copy and paste this command into the terminal:

docker run -d --name wmu_reading_db -e POSTGRES_USER=wmuuser -e POSTGRES_PASSWORD=wmupassword -e POSTGRES_DB=wmu_reading -p 5433:5432 -v wmu_reading_data:/var/lib/postgresql/data postgres:15

Press Enter and wait about 15 seconds before continuing.

Docker is setting up the database in the background.

---

# Part 5 — Load the Reading Content (First Time Only)

This loads the reading standards, stories, and questions into the database. You only do this once.

Run these commands one at a time:

docker cp create_tables.sql wmu_reading_db:/tmp/tables.sql
docker cp k5_reading_prereq.sql wmu_reading_db:/tmp/seed.sql
docker cp modules_seed.sql wmu_reading_db:/tmp/modules.sql
docker cp assessments_seed.sql wmu_reading_db:/tmp/assessments.sql

After that, connect to the database:

docker exec -it wmu_reading_db psql -U wmuuser -d wmu_reading

You should now see:

wmu_reading=#

This means you are inside the database.

Now run these commands one at a time:

\i /tmp/tables.sql
\i /tmp/seed.sql
\i /tmp/modules.sql
\i /tmp/assessments.sql
UPDATE modules SET item_code = module_code, sequence_index = 10;
UPDATE assessments SET sequence_index = 20;

Exit the database with:

\q

---

# Part 6 — Install the Python Dependency (First Time Only)

This installs the tools Python needs to run the demo.

Run:

pip install flask psycopg2-binary

Wait for the installation to finish.

---

# Part 7 — Run the Demo

Run these commands one at a time:

cd reading-demo

python demo.py

You should see a message similar to:

Running on http://localhost:5050

That means the demo is running.

---

# Part 8 — Open the Demo in Your Browser

1. Open any browser (Chrome, Edge, Firefox, etc.)
2. Click the address bar
3. Type:

http://localhost:5050

4. Press Enter

The demo should now appear in your browser.

---

# Using the Demo

## Student Name

Enter any student name in the first box.

## Starting Reading Level

Enter one of these values in the second box:

| Value        | Grade        |
| ------------ | ------------ |
| `K.RSLKID.1` | Kindergarten |
| `1.RSLKID.1` | Grade 1      |
| `2.RSLKID.1` | Grade 2      |
| `3.RSLKID.1` | Grade 3      |
| `4.RSLKID.1` | Grade 4      |
| `5.RSLKID.1` | Grade 5      |

Click **Start** and follow the prompts on screen.

---

# Every Time After the First — Running the Demo Again

You do not need to repeat Parts 4, 5, or 6.

Next time:

1. Open Docker Desktop and wait for “Docker Desktop is running”

2. Open a terminal (`cmd`)

3. Go to the demo folder:
cd C:\path\to\interventive-learning

4. Start the database:
docker start wmu_reading_db

5. Start the demo:
cd reading-demo
python demo.py

6. Open your browser and go to:
http://localhost:5050

---

# How to Stop the Demo

## Stop the Python App

In the terminal running the demo, press:
Ctrl + C

## Stop the Database

Then run:
docker stop wmu_reading_db

---

# Quick Reference

| Action            | Command                                 |
| ----------------- | --------------------------------------- |
| Start database    | `docker start wmu_reading_db`           |
| Go to demo folder | `cd C:\path\to\interventive-learning`   |
| Start demo        | `cd reading-demo` then `python demo.py` |
| Open demo         | Go to `http://localhost:5050`           |
| Stop demo         | Press `Ctrl + C`                        |
| Stop database     | `docker stop wmu_reading_db`            |

/* Testing the webhookv2 */
