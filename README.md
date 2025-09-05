from math import radians, sin, cos, sqrt, atan2

def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    dlat = radians(lat2 - lat1)
    dlon = radians(lon2 - lon1)
    a = sin(dlat/2)**2 + cos(radians(lat1)) * cos(radians(lat2)) * sin(dlon/2)**2
    return R * 2 * atan2(sqrt(a), sqrt(1 - a))
def greedy_tour(dist_matrix, start_index):
    n = len(dist_matrix)
    visited = [False] * n
    path = [start_index]
    visited[start_index] = True

    for _ in range(n - 1):
        last = path[-1]
        next_city = min((i for i in range(n) if not visited[i]), key=lambda i: dist_matrix[last][i])
        path.append(next_city)
        visited[next_city] = True

    return path

def two_opt(path, dist_matrix):
    improved = True
    while improved:
        improved = False
        for i in range(1, len(path) - 2):
            for j in range(i + 1, len(path)):
                if j - i == 1: continue
                new_path = path[:i] + path[i:j][::-1] + path[j:]
                if total_distance(new_path, dist_matrix) < total_distance(path, dist_matrix):
                    path = new_path
                    improved = True
    return path

def total_distance(path, dist_matrix):
    return sum(dist_matrix[path[i]][path[i+1]] for i in range(len(path)-1)) + dist_matrix[path[-1]][path[0]]
import csv, json
from collections import namedtuple
from distance import haversine
from tsp_solver import greedy_tour, two_opt, total_distance

Place = namedtuple("Place", ["name", "lat", "lon"])

def read_places(csv_file):
    with open(csv_file) as f:
        reader = csv.DictReader(f)
        return [Place(row["Name"], float(row["Lat"]), float(row["Lon"])) for row in reader]

def build_matrix(places):
    return [[haversine(p1.lat, p1.lon, p2.lat, p2.lon) for p2 in places] for p1 in places]

def export_geojson(places, path, filename="route.geojson"):
    coords = [[places[i].lon, places[i].lat] for i in path] + [[places[path[0]].lon, places[path[0]].lat]]
    geojson = {
        "type": "FeatureCollection",
        "features": [{
            "type": "Feature",
            "geometry": {
                "type": "LineString",
                "coordinates": coords
            },
            "properties": {}
        }]
    }
    with open(filename, "w") as f:
        json.dump(geojson, f)
