
import os
import time
import datetime
import subprocess
import shutil
import re


"""
Script removes folders in APP_HOME that are older than 7 days
and follow backup format: 'HOME_ (Date and Time)'
----
1. Changes Directory
2. Captures output of command 'ls -ltr'
3. Captures output of 'date "+%D"'  and converts into datetime object 'CURR_DATE'
4. Global list that stores folders that are to be removed
"""

# 1

directory = "/prodlib/TRD/APP_HOME"
os.chdir(directory)

# 2
host = subprocess.Popen(['ls', '-ltr'], stdout=subprocess.PIPE)
output = host.communicate()[0].strip().split("\n")

# 3
child = subprocess.Popen(['date', "+%D"], stdout=subprocess.PIPE)
curr = child.communicate()[0].strip()
CURR_DATE = datetime.datetime(int(str(int(curr[-2:])+2000)),
                int(str(int(curr[0:2]))), int(str(int(curr[3:5]))))

# 4
folders_rm = []


def main():
    """
    1. Adds folders that are to be deleted to global list 'folders_rm'
    2. Removes Folder from folders_rm
    """

    # check if check_name works
    input = 'SCF_HOME_06-07-22_12-03'
    print(check_name(input))

    # 1
    for i in output[1:]:
        fname = i.split()[-1][0:]
        if check_name(fname) and check_time(get_time(fname)):
            folders_rm.append(fname)

    # 2
    for i in folders_rm:
        print(i)
        remove_folder(i)


def check_name(fname):
    """
    Checks if the folder contains '_HOME_' is a directory and
    has a valid date format at back of folder

    Parameters:
    ----
    fname<str>: folder name

    Return:
    ----
    boolean: True if fname fufills conditions, False otherwise
    """

    try:
        dateRegex = re.compile(r'\d\d-\d\d-\d\d_\d\d-\d\d')
        new = str(dateRegex.search(fname))

        if os.path.isdir(fname) and fname.__contains__('_HOME_'):
            if any([char.isdigit() for char in new]):
                return True
        return False
    except:
        print("Error in checking Name")
        return False


def get_time(t):
    """
    Creates datetime object from folder

    Parameters:
    t<str>: folder name

    Return:
    ----
    folder_date<datetime>: datetime object of folder 't', returns False if exception occurs
    """

    try:
        c_time = os.path.getmtime(t)
        folder = time.ctime(c_time)
        folder_date = datetime.datetime.strptime(folder, "%a %b %d %H:%M:%S %Y")
        return folder_date
    except:
        print("Error in getting time")
        return False


def check_time(t):
    """
    Checks if datetime object 't' is older than 7 days

    Parameters:
    ----
    t<datetime>: datetime object of folder

    Return:
    ----
    boolean: True if folder is older than 7 days, False if not or exception occurs
    """

    try:
        return((CURR_DATE - t).days > 7)
    except:
        print("Error in checking time")
        return False


def remove_folder(fname):
    """
    Removes folder 'fname' recursively

    Parameters:
    ----

    fname<str>: name of folder

    Return:
    ----
    NONE unless folder deletion unsuccessful
    """
    try:
        if os.path.isdir(fname):
            shutil.rmtree(fname, ignore_errors=False, onerror=None)
    except:
        print('Error in deleting file')


if __name__ == "__main__":
    main()
