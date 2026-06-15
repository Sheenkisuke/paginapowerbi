# Árbol 1: (id + id) * id
arbol1 = {
    "valor": "*",
    "izquierda": {
        "valor": "+",
        "izquierda": {"valor": "id"},
        "derecha": {"valor": "id"}
    },
    "derecha": {"valor": "id"}
}

# Árbol 2: id + (id * id)
arbol2 = {
    "valor": "+",
    "izquierda": {"valor": "id"},
    "derecha": {
        "valor": "*",
        "izquierda": {"valor": "id"},
        "derecha": {"valor": "id"}
    }
}
