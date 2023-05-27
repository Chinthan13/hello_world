from flask import Flask, request, jsonify

app = Flask(__name__)
@app.route('/students', methods=['GET'])
def get_students():
    page = int(request.args.get('page', 1))
    page_size = int(request.args.get('page_size', 10))
    start_index = (page - 1) * page_size
    end_index = start_index + page_size
    students = load_student_details()
    paginated_students = students[start_index:end_index]
    return jsonify(paginated_students)
@app.route('/students/filter', methods=['POST'])
def filter_students():
    filters = request.get_json()
    students = load_student_details()
    filtered_students = apply_filters(students, filters)

    return jsonify(filtered_students)
def load_student_details():
    # Load student data from a file or any other data source
    # Replace this with your actual implementation
    students = [
        {'id': 1, 'name': 'John Doe', 'total_marks': 85},
        {'id': 2, 'name': 'Jane Smith', 'total_marks': 92},
        {'id': 3, 'name': 'Alex Johnson', 'total_marks': 78},
    ]

    return students
def apply_filters(students, filters):
 
    filtered_students = students

    for filter_key, filter_value in filters.items():
        if filter_key == 'name':
            filtered_students = [s for s in filtered_students if s['name'].lower() == filter_value.lower()]
        elif filter_key == 'total_marks':
            filtered_students = [s for s in filtered_students if s['total_marks'] >= filter_value]

    return filtered_students

if __name__ == '__main__':
    app.run()

