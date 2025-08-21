#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Tab {
    int pageID;
    char url[100];
    struct Tab *prev, *next;
} Tab;

Tab *head = NULL, *current = NULL;
int pageCounter = 1;

// Function to create a new tab
Tab* createTab(char *url) {
    Tab newTab = (Tab)malloc(sizeof(Tab));
    newTab->pageID = pageCounter++;
    strcpy(newTab->url, url);
    newTab->prev = newTab->next = NULL;
    return newTab;
}

// Visit a new page
void visitPage() {
    char url[100];
    printf("Enter URL: ");
    scanf("%s", url);

    Tab *newTab = createTab(url);

    if (head == NULL) {
        head = current = newTab;
    } else {
        current->next = newTab;
        newTab->prev = current;
        current = newTab;
    }
    printf("Opened new tab - PageID: %d, URL: %s\n", current->pageID, current->url);
}

// Move forward
void goForward() {
    if (current != NULL && current->next != NULL) {
        current = current->next;
        printf("Moved forward to PageID: %d, URL: %s\n", current->pageID, current->url);
    } else {
        printf("No next tab available!\n");
    }
}

// Move backward
void goBack() {
    if (current != NULL && current->prev != NULL) {
        current = current->prev;
        printf("Moved back to PageID: %d, URL: %s\n", current->pageID, current->url);
    } else {
        printf("No previous tab available!\n");
    }
}

// Show current tab
void showCurrentTab() {
    if (current != NULL) {
        printf("Current Tab -> PageID: %d, URL: %s\n", current->pageID, current->url);
    } else {
        printf("No tab is open.\n");
    }
}

// Close current tab
void closeCurrentTab() {
    if (current == NULL) {
        printf("No tab to close.\n");
        return;
    }

    Tab *toDelete = current;

    if (current->prev != NULL)
        current->prev->next = current->next;
    if (current->next != NULL)
        current->next->prev = current->prev;

    if (current->next != NULL)
        current = current->next;
    else
        current = current->prev;

    if (toDelete == head)
        head = toDelete->next;

    printf("Closed Tab -> PageID: %d, URL: %s\n", toDelete->pageID, toDelete->url);
    free(toDelete);

    if (current != NULL)
        printf("Now at Tab -> PageID: %d, URL: %s\n", current->pageID, current->url);
    else
        printf("No tabs are open now.\n");
}

// Show history
void showHistory() {
    Tab *temp = head;
    if (temp == NULL) {
        printf("No browsing history.\n");
        return;
    }
    printf("\nBrowsing History:\n");
    while (temp != NULL) {
        printf("PageID: %d, URL: %s\n", temp->pageID, temp->url);
        temp = temp->next;
    }
}

// Menu
int main() {
    int choice;

    while (1) {
        printf("\n--- Browser Menu ---\n");
        printf("1. Visit a new page\n");
        printf("2. Go back\n");
        printf("3. Go forward\n");
        printf("4. Show current tab\n");
        printf("5. Close current tab\n");
        printf("6. Show history\n");
        printf("7. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: visitPage(); break;
            case 2: goBack(); break;
            case 3: goForward(); break;
            case 4: showCurrentTab(); break;
            case 5: closeCurrentTab(); break;
            case 6: showHistory(); break;
            case 7: exit(0);
            default: printf("Invalid choice!\n");
        }
    }
    return 0;
}