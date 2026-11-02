import os
import shutil

# Folder to organize
SOURCE_FOLDER = "Downloads"

FILE_CATEGORIES = {
    "Images": [".jpg", ".jpeg", ".png", ".gif", ".webp"],
    "Videos": [".mp4", ".mkv", ".avi", ".mov"],
    "Documents": [".pdf", ".doc", ".docx", ".txt", ".xlsx", ".pptx"],
    "Archives": [".zip", ".rar", ".7z", ".tar", ".gz"],
    "Audio": [".mp3", ".wav", ".flac", ".aac"],
    "Code": [".py", ".js", ".html", ".css", ".json"]
}

def get_category(extension):
    for category, extensions in FILE_CATEGORIES.items():
        if extension.lower() in extensions:
            return category
    return "Others"


def organize_files():
    if not os.path.exists(SOURCE_FOLDER):
        print(f"Folder not found: {SOURCE_FOLDER}")
        return

    for filename in os.listdir(SOURCE_FOLDER):
        file_path = os.path.join(SOURCE_FOLDER, filename)

        # Skip folders
        if os.path.isdir(file_path):
            continue

        extension = os.path.splitext(filename)[1]
        category = get_category(extension)

        category_folder = os.path.join(SOURCE_FOLDER, category)
        os.makedirs(category_folder, exist_ok=True)

        destination = os.path.join(category_folder, filename)

        # Avoid overwriting existing files
        if os.path.exists(destination):
            base, ext = os.path.splitext(filename)
            counter = 1

            while os.path.exists(destination):
                new_name = f"{base}_{counter}{ext}"
                destination = os.path.join(category_folder, new_name)
                counter += 1

        shutil.move(file_path, destination)

        print(f"Moved: {filename} → {category}/")


if __name__ == "__main__":
    organize_files()
