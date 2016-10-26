### Static functions in helper class:

A class with static functions to help format Texts:

      export class TextFormatterHelper {

        /**
         * Input: "Foo [regex] fizz buzz"
         * Output: " Foo
         *          fizz buzz"
         *
         */
        public static regexToHtmlBr(regex: string, plainText: string): string {  // Static
            return plainText.replace(regex, "<br/>");
        }

        public static linkify(plainText): string { // Static
            let replacedText;
            let replacePattern1;
            let replacePattern2;
            let replacePattern3;

            // URLs starting with http://, https://, or ftp://
            replacePattern1 = /(\b(https?|ftp):\/\/[-A-Z0-9+&@#\/%?=~_|!:,.;]*[-A-Z0-9+&@#\/%=~_|])/gim;
            replacedText = plainText.replace(replacePattern1, '<a href="$1" target="_blank">$1</a>');

            // URLs starting with "www." (without // before it, or it'd re-link the ones done above).
            replacePattern2 = /(^|[^\/])(www\.[\S]+(\b|$))/gim;
            replacedText = replacedText.replace(replacePattern2, '$1<a href="http://$2" target="_blank">$2</a>');

            // Change email addresses to mailto:: links.
            replacePattern3 = /(([a-zA-Z0-9\-\_\.])+@[a-zA-Z\_]+?(\.[a-zA-Z]{2,6})+)/gim;
            replacedText = replacedText.replace(replacePattern3, '<a href="mailto:$1">$1</a>');

            return replacedText;
        }

    }
    
 Use it by calling without initializing:
 
     public parseNotes(text: string) {
         text = TextFormatterService.regexToHtmlBr("/\\n\\n/g", constrNt);
         return TextFormatterService.linkify(text);
     }

### Sort array by string values

Imagine we have an array of TaskItemVO where TaskItemVO looks like:


    export class TaskItemVO {
          public status: string;
    }
    
Use to sort by status 1) "Complete", 2)"In Progress", 3) "On Hold", 4) "Not Started"

    public sortByStatus(): void {

        let sortObj = {
            "Complete": 0,
            "In Progress": 1,
            "On Hold": 2,
            "Not Started": 3,
            "": 4,
            null: 4,
            undefined: 4,
        };

        this.taskItemArray.sort(function (a: TaskItemVO, b: TaskItemVO) {

            if (sortObj[a.status] > sortObj[b.status]) {
               return -1;
            } else if (sortObj[a.status] < sortObj[b.status]) {
               return 1;
            }

            return 0;
        });
    
    }
    
    
 ### Sort by date
 
    public sortByDateField(): void {
        this.selectedTasks.sort((a: ClassX, b: ClassX) => {
             return this.getTime(a.dateField) - this.getTime(b.dateField);
        });
    }

    // deals with null/undefined dates
    private getTime(date?: Date) {
        return date != null ? date.getTime() : 0; 
    }
