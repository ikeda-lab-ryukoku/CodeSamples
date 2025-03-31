using UnityEngine;

[RequireComponent(typeof(MeshFilter), typeof(MeshRenderer), typeof(MeshCollider))]
public class GridMeshGenerator: MonoBehaviour
{
    public int n_divx = 5; // x方向の分割数（セル数）
    public int n_divy = 1; // y方向の分割数（セル数）
    public float width = 5f; // 全体の幅（x方向のサイズ）
    public float height = 1f; // 全体の高さ（y方向のサイズ）
    public Material lineMaterial; // 線を描画するためのマテリアル

    void Start()
    {
        GenerateMesh();
        GenerateGridLines();
    }

    void GenerateMesh()
    {
        int n_vertx = n_divx + 1; // x方向の頂点数
        int n_verty = n_divy + 1; // y方向の頂点数

        // 頂点配列を作成
        Vector3[] vertices = new Vector3[n_vertx * n_verty];
        Vector2[] uv = new Vector2[n_vertx * n_verty];
        float dx = width / n_divx;
        float dy = height / n_divy;

        // 頂点の配置
        for (int y = 0; y < n_verty; y++)
        {
            for (int x = 0; x < n_vertx; x++)
            {
                vertices[y * n_vertx + x] = new Vector3(x * dx, y * dy, 0); // z = 0に固定
                uv[y * n_vertx + x] = new Vector2((float)x / n_divx, (float)y / n_divy);
            }
        }

        // 三角形インデックスを作成
        int[] triangles = new int[6 * n_divx * n_divy]; // 各セルに2つの三角形、各三角形に3つの頂点
        int l = 0;

        for (int y = 0; y < n_divy; y++)
        {
            for (int x = 0; x < n_divx; x++)
            {
                int A = y * n_vertx + x;       // 左上
                int B = A + 1;                 // 右上
                int C = A + n_vertx;           // 左下
                int D = C + 1;                 // 右下

                // 三角形1 (左上、左下、右下)
                triangles[l++] = A;
                triangles[l++] = C;
                triangles[l++] = D;

                // 三角形2 (左上、右上、右下)
                triangles[l++] = A;
                triangles[l++] = D;
                triangles[l++] = B;
            }
        }

        // メッシュを生成
        Mesh mesh = new Mesh();
        mesh.vertices = vertices;
        mesh.triangles = triangles;
        mesh.uv = uv;

        // 法線とバウンディングボックスを再計算
        mesh.RecalculateNormals();
        mesh.RecalculateBounds();

        // メッシュを設定
        MeshFilter meshFilter = GetComponent<MeshFilter>();
        meshFilter.sharedMesh = mesh;

        // コライダーを更新
        MeshCollider meshCollider = GetComponent<MeshCollider>();
        meshCollider.sharedMesh = mesh;
    }

    void GenerateGridLines()
    {
        float dx = width / n_divx;
        float dy = height / n_divy;

        // 横方向の線の描画
        for (int y = 0; y <= n_divy; y++)
        {
            for (int x = 0; x < n_divx; x++)
            {
                Vector3 start = new Vector3(x * dx, y * dy, 0);
                Vector3 end = new Vector3((x + 1) * dx, y * dy, 0);
                DrawLine(start, end);
            }
        }

        // 縦方向の線の描画
        for (int x = 0; x <= n_divx; x++)
        {
            for (int y = 0; y < n_divy; y++)
            {
                Vector3 start = new Vector3(x * dx, y * dy, 0);
                Vector3 end = new Vector3(x * dx, (y + 1) * dy, 0);
                DrawLine(start, end);
            }
        }

        // 対角線の描画
        for (int y = 0; y < n_divy; y++)
        {
            for (int x = 0; x < n_divx; x++)
            {
                Vector3 topLeft = new Vector3(x * dx, (y + 1) * dy, 0);
                Vector3 bottomRight = new Vector3((x + 1) * dx, y * dy, 0);
                DrawLine(topLeft, bottomRight);
            }
        }
    }

    // 点をつないだ線を描画
    void DrawLine(Vector3 start, Vector3 end)
    {
        GameObject lineObj = new GameObject("Line");
        lineObj.transform.SetParent(transform);

        LineRenderer lineRenderer = lineObj.AddComponent<LineRenderer>();
        lineRenderer.material = lineMaterial;
        lineRenderer.positionCount = 2;
        lineRenderer.startWidth = 0.05f; // 線の太さを調整
        lineRenderer.endWidth = 0.05f;
        lineRenderer.SetPosition(0, start);
        lineRenderer.SetPosition(1, end);
        lineRenderer.useWorldSpace = false; // ローカル空間で動作させる
    }
}
