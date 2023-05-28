# Dynamic Array

A dynamic array is a data structure that is often used when the number of elements, that must be stored, is unknown during compile time. A Dynamic Array is especially convenient for **searching** based on a given index and **insertion & deletion** at the **back** of the Array.

## Characteristics

The time complexity is defined:
- Insertion:
    - O(n), in case shifting or resizing is required, which might be applicable to part of array:
        - Front
        - Middle
        - Back 
    - O(1), in case no shifting and resizing is required, which might be applicable to part of array:
        - Back
- Deletion:
    - O(n), in case shifting or resizing is required, which might be applicable to part of array:
        - Front
        - Middle
        - Back
    - O(1), in case no shifting or resizing is required, which might be applicable to part of array:
        - Back
- Search:
    - O(n), in case the index of the searched element is unknown
    - O(1), in case the index of the searched element is known

The space complexity is defined:
- Insertion:
    - O(n), in case shifting or resizing is required, which might be applicable to part of array:
        - Front
        - Middle
        - Back
    - O(1), in case no shifting and resizing is required, which might be applicable to part of array:
        - Back
- Deletion:
    - O(n), in case shifting or resizing is required, which might be applicable to part of array:
        - Front
        - Middle
        - Back
    - O(1), in case no shifting or resizing is required, which might be applicable to part of array:
        - Back
- Search:
    - O(1)

## Applicability

Standard language applications:
- std::Vector in C++

## Implementation
<code>

    class Vector {
    public:
        void push_back(int* value)
        {
            if ((m_index + 1) > size) {
                int** temp = new int* [size * 2];
                for (int n = 0; n < size; n++)
                {
                    temp[n] = data[n];
                }
                delete[] data;
                data = temp;
                size *= 2;
            }

            data[m_index] = value;
            m_index++;
        }

        void pop_back() 
        {   
            m_index--;
        }

        void insert_at(int index, int* value)
        {
            int** temp = new int* [size+1];

            for (int n = 0; n < index; n++) {
                temp[n] = data[n];
            }
            
            temp[index] = value;
            
            for (int n = index; n < size; n++) {
                temp[n + 1] = data[n];
            }
            
            delete[] data;
            data = temp;
            size = size + 1;
            m_index++;
        }

        int* get_at(int index) {
            return data[index];
        }

        void delete_at(int index) 
        {
            int** temp = new int* [size];
            
            for (int n = 0; n < index; n++) {
                temp[n] = data[n];        
            }
            
            delete data[index];

            for (int n = index; n < size; n++) {
                temp[n] = data[n+1];
            }

            delete[] data;
            data = temp;
            m_index--;
        }

        void Print() {
            for (int n = 0; n < m_index; n++) {
                std::cout << *data[n] << std::endl;
            }
        }

    private:
        int size = 1;
        int m_index = 0;
        int** data = new int* [size];
    };

    int main()
    {
        Vector* vector = new Vector();
        vector->push_back( new int(1));
        vector->push_back(new int(3));
        vector->push_back(new int(4));
        vector->insert_at(1, new int(2));
        vector->Print();
        vector->delete_at(1);
        vector->Print();
        vector->push_back(new int(4));
        vector->Print();
    }
</code>
